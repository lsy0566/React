- **mapStateToProps()**

  ```javascript
  // mapStateToProps는 기본적으로 state가 첫번째 인자로 사용된다.
  // 그로인해 우리는 state를 다룰수 있게된다.
  const mapStateToProps = state => ({ todos: state.todos })
  ```

  

- /src/actions/index.js

  ```javascript
  import axios from 'axios';
  
  const API_KEY = '76603f01ced67bd4cb8097b13bfa5ced';
  const ROOT_URL = `https://api.openweathermap.org/data/2.5/forecast?appid=${API_KEY}`;
  
  export const FETCH_WEATHER = 'FETCH_WEATHER'
  
  // redux action
  // type (mandatory) 필수
  // payload (optional, data)
  
  export function fetchWeather(city) {
      const url = `${ROOT_URL}&q=${city}`
      const request = axios.get(url);
  
      return {
          type: FETCH_WEATHER,
          payload: request
      }
  }
  
  
  ```

  

- /src/components/chart.js

  ```javascript
  import _ from 'lodash';
  import React from 'react';
  import { Sparklines, SparklinesLine } from 'react-sparklines';
  import { SparklinesReferenceLine } from 'react-sparklines';
  
  export default (props) => {
    return (
      <div>
        <Sparklines width={180} height={180} 
        data={props.data}>
          <SparklinesLine color={props.color}/>
          <SparklinesReferenceLine type="avg"/>
        </Sparklines>
      </div>
    );
  };
  ```

  

- /src/containers/search_bar.js

  ```javascript
  import React, { Component } from 'react';
  import { connect } from 'react-redux'
  import { bindActionCreators } from 'redux';
  import { fetchWeather } from '../actions/index';
  
  class SearchBar extends Component {
    constructor(props) {
      super(props);
  
      this.state = { term: '' };
  
      this.onInputChange = this.onInputChange.bind(this);
      this.onFormSubmit = this.onFormSubmit.bind(this);
  
    }
  
    onInputChange(event) {
      this.setState({
        term: event.target.value
      })
    }
  
    onFormSubmit(event) {
      event.preventDefault();
      this.props.fetchWeather(this.state.term);
      this.setState({ //검색이 되면 원래 있던 term값 제거
        term: ''
      })
    }
  
    render() {
      return (
        <form onSubmit={this.onFormSubmit} className="input-group">
          <input 
          placeholder="Get a five-day forecast in your favorite cities"
          className="form-control"
          value={this.state.term}
          onChange={this.onInputChange}
            />
          <span className="input-group-btn">
            <button type="submit" className="btn btn-secondary">Submit</button>
          </span>
        </form>
      );
    }
  }
  
  // mapDispatchToProps function
  function mapDispatchToProps(dispatch) {
    return bindActionCreators({ fetchWeather }, dispatch);
  }
  
  // connect mapping
  // mapStateToprops,
  export default connect(null, mapDispatchToProps)(SearchBar);
  ```

  

- /src/containers/weather_list.js

  ```javascript
  import React, { Component } from 'react';
  import { connect } from 'react-redux';
  import Chart from '../components/chart';
  
  class WeatherList extends Component {
    renderWeather(cityData) {
      console.log(cityData)
      const name = cityData.city.name;
      const country = cityData.city.country;
      const temps = cityData.list.map(weather => weather.main.temp);
      const pressures = cityData.list.map(weather => weather.main.pressure);
      const humidities = cityData.list.map(weather => weather.main.humidity);
  
      return (
        <tr key={name}>
          <td>{name},{country}</td>
          <td><Chart data={temps} color="orange"/></td>
          <td><Chart data={pressures} color="green"/></td>
          <td><Chart data={humidities} color="black"/></td>
        </tr>
      )
    }
  
    render() {
      return (
        <table className="td, th">
          <thead>
            <tr className="th">
              <th>City</th>
              <th>Temperature</th>
              <th>Pressure</th>
              <th>himidity</th>
            </tr>
          </thead>
          <tbody>
            {this.props.weather.map(this.renderWeather)}
          </tbody>
        </table>
      );
    }
  }
  
  // mapSrtateToProps funciton
  function mapStateToProps(state) {
    return { weather: state.weather };
  }
  
  // connect mapping
  export default connect(mapStateToProps)(WeatherList);
  ```

  

- /src/reducers/index.js

  ```javascript
  import { combineReducers } from 'redux';
  import WeatherReducer from './reducer_weather';
  
  const rootReducer = combineReducers({
      weather: WeatherReducer
  });
  
  export default rootReducer;
  
  ```

  

- /src/reducers/reducer_weather.js

  ```javascript
  import { FETCH_WEATHER } from "../actions";
  
  // biz logic
  // src/reducers/reducer_weather.js
  // action.type(FETCH_WEATHER), action.payload(data = weather json)
  export default function(state = [], action) {
      switch(action.type) {
          case FETCH_WEATHER:
              // (X)return state.push(action.payload.data); 자바스크립트에서 배열에 추가할 때
              // (old)return state.concat(action.payload.data);
              return [ action.payload.data, ...state ]
          default:
              return state;
      }
  }
  ```

  

- ./App.js

  ```javascript
  import React, { Component } from 'react';
  import './App.css';
  
  import SearchBar from './containers/search_bar';
  import WeatherList from './containers/weather_list';
  
  export default class App extends Component {
    render() {
      return (
        <div>
          <SearchBar/>
          <WeatherList/>
        </div>
      );
    }
  }
  
  ```

  