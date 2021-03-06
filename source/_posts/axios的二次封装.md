---
title: axios的二次封装
date: 2018-07-11 16:08:17
tags: axios
categories: http
---

### 一切尽在码中

#### src/api/axios.config.js  axios配置

<!-- more -->

```javascript
import axios from 'axios'
import {
  defaultTimeout,
  defaultContentType,
  statusCodeMap
} from 'shared/constants-define'

axios.defaults.timeout = defaultTimeout
axios.defaults.headers.post['Content-Type'] = defaultContentType

axios.interceptors.request.use(
  config => {
    if (!window.navigator.onLine) {
      console.log('网络请求失败，请检查您的网络设置')
      return Promise.resolve({ msg: '网络请求失败，请检查您的网络设置' })
    }
    return config
  },
  err => {
    console.log('req_err: ', err)
    return Promise.reject(err)
  }
)

axios.interceptors.response.use(
  res => {
    return res
  },
  err => {
    console.log('res_err: ', err)
    if (err.code === 'ECONNABORTED') {
      console.dir(err)
      console.log('请求超时')
      return Promise.reject(err)
    }
    if (!err) {
      return
    }
    const statusCode = err.response.status
    console.log(statusCodeMap[statusCode] || '未知网络请求错误')
    return Promise.reject(err)
  }
)

const get = async (url, params = {}, headers = {}) => {
  const res = await axios.get(url, { params, headers })
  return Promise.resolve(res)
}

const post = async (url, data) => {
  const res = await axios({
    method: 'post',
    url,
    data
  })
  return Promise.resolve(res)
}

export default { get, post }

```

#### src/shared/consts-define.js  axios配置所需的常量定义

```javascript
export const defaultTimeout = 10000

export const defaultContentType = 'application/x-www-form-urlencoded;charset=UTF-8'

export const statusCodeMap = {
  301: '永久重定向',
  400: '请求语法错误',
  401: '未授权',
  403: '禁止访问',
  404: '没有资源',
  500: '服务器错误',
  503: '服务器错误'
}

```

#### src/api/user.js  调用axios

```javascript
import $ from 'api/axios.config'

export function getUserIntegral (headers) {
  return $.get('/webcast/userIntegral', {}, headers)
}

```