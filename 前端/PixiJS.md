- 创建 HTML 文件
- 使用 Web 服务器提供文件
- 加载 PixiJS 库
- 创建[应用程序](https://pixijs.download/release/docs/PIXI.Application.html)
- 将生成的视图添加到 DOM
- 将图像添加到舞台
- 编写更新循环





创建一个动画的流程大概是这样的：

- 创建舞台
- 创建画布
- 把画布挂载到DOM上
- 创建精灵
- 显示精灵
- 操作精灵



要想把元素参与动画中，必须加入舞台。

只有在舞台上的画布上的元素，才可被渲染。



舞台、画布搭建好就可以简单的进行动画渲染。







## PIXI.Application（舞台、画布）

####  new PIXI.Application (options)

| Name                            | Type                                                         | Attributes | Default                  | Description                                                  |
| :------------------------------ | :----------------------------------------------------------- | :--------- | :----------------------- | :----------------------------------------------------------- |
| `options`                       | object                                                       | <optional> |                          | 可选渲染器参数。                                             |
| `options.autoStart`             | boolean                                                      | <optional> | true                     | Automatically starts the rendering after the construction. **Note**: Setting this parameter to false does NOT stop the shared ticker even if you set options.sharedTicker to true in case that it is already started. Stop it by your own. |
| `options.width`                 | number                                                       | <optional> | 800                      | 渲染页面宽度                                                 |
| `options.height`                | number                                                       | <optional> | 600                      | 渲染页面高度                                                 |
| `options.view`                  | [PIXI.ICanvas](https://pixijs.download/release/docs/PIXI.ICanvas.html) | <optional> |                          | 渲染页面结果，可作为 Element 通过 `addChild()` 参数显示      |
| `options.useContextAlpha`       | boolean                                                      | <optional> | true                     | 传递canvasContext 的 Alpha 属性的值。如果您想设置透明度，请使用“backgroundAlpha”。此选项适用于需要不透明的情况，可能适用于某些旧设备。 |
| `options.autoDensity`           | boolean                                                      | <optional> | false                    | Resize render 将查看CSS像素，以允许分辨率低于1。             |
| `options.antialias`             | boolean                                                      | <optional> | false                    | Sets 抗锯齿                                                  |
| `options.preserveDrawingBuffer` | boolean                                                      | <optional> | false                    | Enables drawing buffer preservation, enable this if you need to call toDataURL on the WebGL context. |
| `options.resolution`            | number                                                       | <optional> | PIXI.settings.RESOLUTION | The resolution / device pixel ratio of the renderer.         |
| `options.forceCanvas`           | boolean                                                      | <optional> | false                    | prevents selection of WebGL renderer, even if such is present, this option only is available when using **pixi.js-legacy** or **@pixi/canvas-renderer** modules, otherwise it is ignored. |
| `options.backgroundColor`       | number \| string                                             | <optional> | 0x000000                 | 设置渲染地区的背景颜色（非透明）                             |
| `options.background`            | number \| string                                             | <optional> |                          | `options.backgroundColor` 的别名。                           |
| `options.backgroundAlpha`       | number                                                       | <optional> | 1                        | 值从0（全半透明）到1（全透明）。                             |
| `options.clearBeforeRender`     | boolean                                                      | <optional> | true                     | This sets if the renderer will clear the canvas or not before the new render pass. |
| `options.powerPreference`       | string                                                       | <optional> |                          | Parameter passed to webgl context, set to "high-performance" for devices with dual graphics card. **(WebGL only)**. |
| `options.sharedTicker`          | boolean                                                      | <optional> | false                    | `true` to use PIXI.Ticker.shared, `false` to create new ticker. If set to false, you cannot register a handler to occur before anything that runs on the shared ticker. The system ticker will always run before both the shared ticker and the app ticker. |
| `options.sharedLoader`          | boolean                                                      | <optional> | false                    | `true` to use PIXI.Loader.shared, `false` to create new Loader. |
| `options.resizeTo`              | Window \| HTMLElement                                        | <optional> |                          | 窗口大小自适应                                               |
| `options.hello`                 | boolean                                                      | <optional> | false                    | Logs renderer type and version.                              |



### Members

####  stage [PIXI.Container](https://pixijs.download/release/docs/PIXI.Container.html)

舞台对象

~~~js
const particleContainer = new PIXI.Container();

//想要让元素被渲染，加入到舞台中
app.stage.addChild(particleContainer);
~~~



####  renderer [PIXI.Renderer](https://pixijs.download/release/docs/PIXI.Renderer.html) | [PIXI.CanvasRenderer](https://pixijs.download/release/docs/PIXI.CanvasRenderer.html)

画布对象，负责进行渲染。

WebGL renderer if available, otherwise CanvasRenderer.



#### view [PIXI.ICanvas](https://pixijs.download/release/docs/PIXI.ICanvas.html) readonly

渲染后的结果，是一个 Element。

eg：可以作为子元素传入一个 div 中展示

~~~js
document.querySelector('#app').appendChild(app.view);
~~~







####  PIXI.Application._plugins [PIXI.IApplicationPlugin](https://pixijs.download/release/docs/PIXI.IApplicationPlugin.html)[] static

Collection of installed plugins.



####  resizeTo Window | HTMLElement

内容自适应浏览器屏幕大小。



####  screen [PIXI.Rectangle](https://pixijs.download/release/docs/PIXI.Rectangle.html) readonly

Reference to the renderer's screen rectangle. Its safe to use as `filterArea` or `hitArea` for the whole screen.



####  ticker [PIXI.Ticker](https://pixijs.download/release/docs/PIXI.Ticker.html)

Ticker for doing render updates.

- Default Value:

  PIXI.Ticker.shared





### Methods

####  destroy (removeView, stageOptions)void

[Application.ts:142](https://pixijs.download/release/docs/packages_app_src_Application.ts.html)

Destroy and don't use after this.

| Name                       | Type              | Attributes | Default | Description                                                  |
| :------------------------- | :---------------- | :--------- | :------ | :----------------------------------------------------------- |
| `removeView`               | boolean           | <optional> | false   | Automatically remove canvas from DOM.                        |
| `stageOptions`             | object \| boolean | <optional> |         | Options parameter. A boolean will act as if all options have been set to that value |
| `stageOptions.children`    | boolean           | <optional> | false   | if set to true, all the children will have their destroy method called as well. 'stageOptions' will be passed on to those calls. |
| `stageOptions.texture`     | boolean           | <optional> | false   | Only used for child Sprites if stageOptions.children is set to true. Should it destroy the texture of the child sprite |
| `stageOptions.baseTexture` | boolean           | <optional> | false   | Only used for child Sprites if stageOptions.children is set to true. Should it destroy the base texture of the child sprite |



####  render ()void

[Application.ts:116](https://pixijs.download/release/docs/packages_app_src_Application.ts.html)

Render the current stage.





####  resize ()void

[ResizePlugin.ts:94](https://pixijs.download/release/docs/packages_app_src_ResizePlugin.ts.html)

Execute an immediate resize on the renderer, this is not throttled and can be expensive to call many times in a row. Will resize only if `resizeTo` property is set.



####  start ()void

[TickerPlugin.ts:67](https://pixijs.download/release/docs/packages_ticker_src_TickerPlugin.ts.html)

Convenience method for starting the render.



####  stop ()void

[TickerPlugin.ts:56](https://pixijs.download/release/docs/packages_ticker_src_TickerPlugin.ts.html)

Convenience method for stopping the render.