---
title: OAuth 2.0 in the real world
date: 2019-10-21 15:36:24
categories: [OAuth 2.0 in action]
tags: [OAuth2.0, notes]
---

Index: OAuth 2.0 in action - chapter 6.1

## Implicit grant type

客户端完全运行在浏览器中。

![https://i.quantuminit.com/21761a033c5b4ed9.jpg](https://i.quantuminit.com/21761a033c5b4ed9.jpg)

它具有严重的局限性：

由于私隐对于浏览器本身来说是可用，所以对于使用这个工作流的客户端来说，这个客户端没有用于保存客户端隐私的实际的方法。这个工作流只使用授权端点并且不使用token端点，这个局限性不影响它的功能，以为客户端从不需要在授权端点进行验证。然而，缺乏任何验证客户端的方式影响到了授权类型的安全，并且应该小心使用。这个授权类型不能够用于获取一个refresh token。

客户端使用HTTP POST方法，想服务端发送请求，请求的URL中包含response_type，并且使用表单提交数据。

服务端代码实例：

```javascript
var express = require("express");
var app = express();
var nosql = require("nosql");

app.post("/token", function(req, res) {
    if (query.response_type == 'token') {
        /*
         * Implement response_type=token here
         */
        var rscope = getScopesFromForm(req.body);
        var client = getClient(query.client_id);
        var cscope = client.scope ? client.scope.split(' ') : undefined;
        if (__.difference(rscope, cscope).length > 0) {
            var urlParsed = buildUrl(query.redirect_uri,
                {},
                qs.stringify({error: 'invalid_scope'})
            );
            res.redirect(urlParsed);
            return;
        }
        var access_token = randomstring.generate();
        var auth = req.headers['authorization'];
        if (auth) {
            var clientCredentials = decodeClientCredentials(auth);
            var clientId = clientCredentials.id;
        }
        nosql.insert({ access_token: access_token, clinet_id: clientId, scope: rscope });
        var token_response = { access_token: access_token, token_type: 'Bearer', scope: rscope.join(' ') };
        if (query.state) {
            token_response.state = query.state;
        }
        var urlParsed = buildUrl(query.redirect_uri,
            {},
            qs.stringify(token_response)
        );
        res.redirect(urlParsed);
        return;

    }
});
```

## Client Credentials grant type

倘若没有明显的资源拥有者，或资源拥有者不能够与客户端软件本身分别出。这是相当常见的情况，在这种情况中，后端系统需要直接与彼此进行通信并且没必要代表任何一个特定的用户。

![https://i.quantuminit.com/218c305cbfe04e1e.jpg](https://i.quantuminit.com/218c305cbfe04e1e.jpg)

客户端代表自己（自己就是资源拥有者）从token端点获取access token。

客户端使用HTTP POST方法，向服务端发送含有表单数据的请求。

服务端代码实例：


```javascript
var express = require("express");
var app = express();
var nosql = require("nosql");

app.post('/token', function(req, res) {
    if (req.body.grant_type == 'client_credentials') {
        /*
         * Implement the client credentials grant type
         */
        var rscope = req.body.scope ? req.body.scope.split(' ') : undefined;
        var cscope = client.scope ? client.scope.split(' ') : undefined;
        if (__.difference(rscope, cscope).length > 0) {
            res.status(400).json({error: 'invalid_scope'});
            return;
        }

        var access_token = randomstring.generate();
        var token_response = { access_token: access_token, token_type: 'Bearer', scope: rscope.join(' ') };
        nosql.insert({ access_token: access_token, client_id: clientId, scope: rscope });
        res.status(200).json(token_response);
        return;
    }
});
```

客户端代码实例：

```javascript
var express = require("express");
var app = express();

var authServer = {
    tokenEndpoint: "http://localhost:9001/token"
}

app.get('/authorize', function(req, res){
    access_token = null;
    scope = null;

    /*
     * Implement the client credentials flow here
     */

    var form_data = qs.stringify({
        grant_type: 'client_credentials',
        scope: client.scope
    });

    var headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Authorization': 'Basic ' + encodeClientCredentials(client.client_id, client.client_secret),
    };

    var tokRes = request('POST', authServer.tokenEndpoint, {
        body: form_data,
        headers: headers
    });

    if (tokRes.statusCode >= 200 && tokRes.statusCode < 300) {
        var body = JSON.parse(tokRes.getBody());
        access_token = body.access_token;
        scope = body.scope;
        res.render('index', {access_token: access_token, scope: scope});
    } else {
        res.render('error', {error: 'Unable to fetch access token, server response: ' + tokRes.statusCode});
    }
});

```

## Resource owner credentials grant type

假设资源拥有者在授权服务器上拥有普通的用户名以及密码，那么对于客户端来说，能够提示用户使用这一凭证并且使用它们来交换获取一个access token。resource owner credentials 授权类型，也称密码工作流，允许一个客户端仅仅这么做。

![https://i.quantuminit.com/20039fb08a954b51.png](https://i.quantuminit.com/20039fb08a954b51.png)

服务端代码实例：

```javascript
var express = require("express");
var app = express();
var nosql = require("nosql");

app.post("/token", function(req, res) {

    if (req.body.grant_type == 'password') {
     
        /*
         * Implement the resource owner credentials grant type
         */
        var getUser = function(username) {  
            return userInfo[username];          
        }
        var username = req.body.username;   
        var user = getUser(username);       
        if (!user) {
            // The user does not exist.         
            res.status(401).json({error: 'invalid_grant'});
            return;
        }
        var password = req.body.password;
        if (user.password != password) {
            res.status(401).json({error: 'invalid_granty'});
            return;
        }
        var rscope = req.body.scope ? req.body.scope.split(' ') : undefined;
        var cscope = client.scope ? client.scope.split(' ') : undefined;
        if (__.difference(rscope, cscope).length > 0) {
            res.status(401).json({error: 'invalid_scope'});
            return;
        }
        var access_token = randomstring.generate();
        var refresh_token = randomstring.generate();
     
        nosql.insert({ access_token: access_token, client_id: clientId, scope: rscope });
        nosql.insert({ refresh_token: refresh_token, client_id: clientId, scope: rscope });
     
        var token_response = {access_token: access_token, token_type: 'Bearer', refresh_token: refresh_token, scope: rscope.join(' ')};
     
        res.status(200).json(token_response);
     
    }
});
```

客户端代码实例：

```javascript
var express = require("express");
var app = express();

app.post('/username_password', function(req, res) {
	var username = req.body.username;
	var password = req.body.password;
	
	var form_data = qs.stringify({
		grant_type: 'password',
		username: username,
		password: password,
		scope: client.scope
	});
	
	var headers = {
		'Content-Type': 'application/x-www-form-urlencoded',
		'Authorization': 'Basic ' + encodeClientCredentials(client.client_id, client.client_secret)
	};

	var tokRes = request('POST', authServer.tokenEndpoint, {	
		body: form_data,
		headers: headers
	});
	
	if (tokRes.statusCode >= 200 && tokRes.statusCode < 300) {
		var body = JSON.parse(tokRes.getBody());
	
		access_token = body.access_token;

		scope = body.scope;

		refresh_token = body.refresh_token;

		res.render('index', {access_token: access_token, refresh_token: refresh_token, scope: scope});
	} else {
		res.render('error', {error: 'Unable to fetch access token, server response: ' + tokRes.statusCode})
	}
});
```

## Assertion grant types

OAuth工作组发布的第一个授权类型，断言授权类型，客户端给出一个结构化并且加密保护的项，该项称为一个*断言*，该项发送到授权服务器用来交换一个token。可以将端口当作一个认证后的文档，比如证书。到目前为止有两种格式是标准化的：一个使用Security Assertion Markup languate（SAML）和另一个使用JSON Web Token（JWT）。这个授权类型不包括使用反向信道。

![https://i.quantuminit.com/e602f36e072343b9.png](https://i.quantuminit.com/e602f36e072343b9.png)

客户端发送给授权服务器的assertion：

![https://i.quantuminit.com/054e0de15b4a4762.png](https://i.quantuminit.com/054e0de15b4a4762.png)

对assertion的主体进行解密之后：

![https://i.quantuminit.com/f5810bd42b9b41d7.png](https://i.quantuminit.com/f5810bd42b9b41d7.png)

授权服务器解析 assertion，检查它的加密保护措施以及通过处理它的内容来决定生成什么类型的token。这个 assertion 可以表示任意数量的不同的事情，比如资源拥有者的身份或一系列允许的域（权限）。

## Choosing the appropriate grant type

![https://i.quantuminit.com/96a515de7dbc4443.svg](https://i.quantuminit.com/96a515de7dbc4443.svg)

## 小小的总结

- Implicit grants 可以用户没有单独客户端的在浏览器中的应用程序
- Client credentials grants 以及 assertion grants 能够用于没有明显的资源拥有者的服务端应用程序
- 不到万不得已，都不应该使用 resource owner credentials grant（也就是密码）。

