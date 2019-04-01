# Mozilla WebThings - OpenAPI version

`swagger-editor` to create/edit OpenAPI version of Web Connected Lamp (Example 2) from [Mozilla WoT](https://iot.mozilla.org/wot/)

GitHub Page : [phyunsj.github.io/web-thing-swagger-page](https://phyunsj.github.io/web-thing-swagger-page)

<p align="left">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/wot-openapi-swagger-editor.png" width="700px"/>
</p>

## Node-RED REST Server

<p align="center">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/wot-node-red.png" width="750px"/>
</p>

Instead of creating multiple API endpoints (mutiple http nodes. looks messy :cyclone: ), consolidate a single http node using `all()` routing method from `express`. (`RED.httpNode` is an instance of `express`) 

- `nodes/core/io/21-httpin.html`

```
  <div class="form-row">
      <label for="node-input-method"><i class="fa fa-tasks"></i> <span data-i18n="httpin.label.method"></span></label>
      <select type="text" id="node-input-method" style="width:70%;">
      <option value="get">GET</option>
      <option value="post">POST</option>
      <option value="put">PUT</option>
      <option value="delete">DELETE</option>
      <option value="patch">PATCH</option>
+     <option value="all">ALL</option>
      </select>
  </div>
```

- `nodes/core/io/21-httpin.js`

```
function HTTPIn(n) {
  ...
  this.callback = function(req,res) {
     var msgid = RED.util.generateId();
     res._msgid = msgid;
     if (node.method.match(/^(post|delete|put|options|patch)$/)) {
         node.send({_msgid:msgid,req:req,res:createResponseWrapper(node,res),payload:req.body});
     } else if (node.method == "get") {
         node.send({_msgid:msgid,req:req,res:createResponseWrapper(node,res),payload:req.query});
+    } else if (node.method == "all")  {
+        if ( req.method.match(/^(POST|DELETE|PUT|OPTIONS|PATCH)$/)) {
+            node.send({_msgid:msgid,req:req,res:createResponseWrapper(node,res),payload:req.body});
+        } else if (req.method == "GET") {
+            node.send({_msgid:msgid,req:req,res:createResponseWrapper(node,res),payload:req.query});
+        }
    } else { 
        node.send({_msgid:msgid,req:req,res:createResponseWrapper(node,res)});
    }
  };          
  ...        
  if (this.method == "get") {
  ...     
+ } else if (this.method == "all") {
+     RED.httpNode.all(this.url,cookieParser(),...,this.callback,this.errorHandler);
+ } 
  ...
}
```

- `nodes/core/core/80-function.js`

```
function FunctionNode(n) {
   ...
   var sandbox = {
            console:console,
            util:util,
            Buffer:Buffer,
            Date: Date,
            RED: {
                util: RED.util
            },
+            require : require,
   ...
}
```

## Host swagger editor (dist version) on GitHub Page

1. create a repository
2. generate `wot.yaml`
3. index.html

```
    const editor = SwaggerEditorBundle({
 +    url: 'wot.yaml',
      dom_id: '#swagger-editor',
      layout: 'StandaloneLayout',
      presets: [
        SwaggerEditorStandalonePreset
      ]
    })
```
4. upload `index.html` & `wot.yaml` & `dist/*`
5. GitHub Repository Settings

<p align="left">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/github-page-setting.png" width="300px"/>
</p>

6. visit `https://<your-name>.github.io/<repository-name>`

####  Related Posts

- [Mozilla WebThing API Specification] (https://iot.mozilla.org/wot/)
- [Documenting APIs: A guide for technical writers and engineers](https://idratherbewriting.com/learnapidoc/)
- [Swagger](https://swagger.io/)
- [swagger-editor](https://github.com/swagger-api/swagger-editor)
- [AsyncAPI - Messsage Drive API](https://www.asyncapi.com/)
- [Run Chrome browser without CORS](https://alfilatov.com/posts/run-chrome-without-cors/)
