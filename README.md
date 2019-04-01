# Mozilla WebThings - OpenAPI version

`swagger-editor` to create/edit OpenAPI version of Web Connected Lamp (Example 2) from [Mozilla WoT] (https://iot.mozilla.org/wot/)

GitHub Page : [web-thing-swagger-page](https://phyunsj.github.io/web-thing-swagger-page)

<p align="left">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/wot-openapi-swagger-editor.png" width="700px"/>
</p>

## Node-RED REST Server

<p align="left">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/wot-node-red.png" width="700px"/>
</p>

## Host swagger editor (dist version) on GitHub Page

1. create a repository
2. generate `wot.yaml`
3. upload `index.html` & `wot.yaml` & `dist/*`
4. index.html

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
5. Settings

<p align="left">
<img src="https://github.com/phyunsj/web-thing-swagger-page/blob/master/images/github-page-setting.png" width="300px"/>
</p>

6. visit `https://<your-name>.github.io/<repository-name>`

####  Related Posts

- [Mozilla WebThing API Specification] (https://iot.mozilla.org/wot/)
- [Swagger](https://swagger.io/)
- [swagger-editor](https://github.com/swagger-api/swagger-editor)
- [AsyncAPI - Messsage Drive API](https://www.asyncapi.com/)
