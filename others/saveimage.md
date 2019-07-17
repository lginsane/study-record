# html 转 base64 图片并保存到本地

* [html2canvas](http://html2canvas.hertzen.com/) 将 html 转化为 canvas
  * 下载 `npm install --save html2canvas`
  * 引入 `import html2canvas from 'html2canvas'`
  * 使用 
    ``` 简单使用
      conversionImg(id) {
        html2canvas(document.getElementById(id)).then(canvas => {
          const imgUrl = canvas.toDataURL('image/png')
        })
      }
      ```
      ``` 配置使用
      conversionImg(id) {
        const elem = document.getElementById(id)
        const elemWidth = elem.offsetWidth
        const elemHeight = elem.offsetHeight
        const canvas = document.createElement('canvas')
        const scale = 2
        canvas.width = elemWidth * scale
        canvas.height = elemHeight * scale
        canvas.getContext('2d').scale(scale, scale)
        const opts = {
          scale: scale, // 添加的scale 参数
          canvas: canvas, // 自定义 canvas
          // logging: true, // 日志开关，便于查看html2canvas的内部执行流程
          width: elemWidth, // dom 原始宽度
          height: elemHeight,
          useCORS: true // 开启跨域配置
        }
        html2canvas(elem, opts).then(canvas => {
          const imgUrl = canvas.toDataURL('image/png')
        })
      }
      ```
* canvas.toDataURL('image/png')将 canvas 转化为 base64 图片，`image/png`为图片到格式
  ```
    const imgUrl = canvas.toDataURL('image/png')
  ```
* 通过a标签的`download`属性下载图片
  * `fileName` 为保存到本地到名字，`data`为base64(data:image/png;base64,iVBO...)

  ```
  downloadImg(fileName, imgUrl) {
    const save_link = document.createElementNS('http://www.w3.org/1999/xhtml', 'a')
    save_link.href = imgUrl
    save_link.download = fileName
    var event=document.createEvent('MouseEvents')
    event.initMouseEvent('click',true,false,window,0,0,0,0,0,false,false,false,false,0,null)
    save_link.dispatchEvent(event)
  }
  ```

