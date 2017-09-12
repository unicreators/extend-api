## extend-api

extend api.


## Install

```sh
$ npm install extend-api
```


## Usage

### Custom api


```js

const { Api, ApiExtend } = require('extend-api');

let CustomApi = class customApi extends Api {
    constructor(token) { super(); this.token = token; }
    // override
    async buildApiReqOpts(extendInvokeOpts) {
        // attach 'token'
        extendInvokeOpts.qs = Object.assign({ token: this.token }, extendInvokeOpts.qs);
        return extendInvokeOpts;
    }
};


```


### Custom extend


```js


let MessageExtend = class MessageExtend extends ApiExtend {
    async send(to, content) {
        return await this.invoke(
            'https://api.weixin.qq.com/cgi-bin/message/custom/send',
            // request opts.   
            // (see: https://github.com/request/request#requestoptions-callback)
            {
                body: {
                    touser: to, msgtype: 'text',
                    text: { content }
                }
            }, 'POST');
    }
};


```


### Register extend


```js


let api = new CustomApi('token..');


// register
api.extend('message', MessageExtend);


```



### Use


```js


api.message.send('to', 'content')
    .then(function (result) {
        // ..
    }).catch(function (err) {
        // ..
    });

    
// or
// await api.message.send('openId', 'content');


```






### License

[MIT](LICENSE)