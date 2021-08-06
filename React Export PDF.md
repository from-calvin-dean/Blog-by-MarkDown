---
title: React Export PDF
date: 2021-08-06 18:52:59
tags:
---

### 导出pdf

1. 下载并导入html2pdf

   ```js
   import html2pdf from 'html2pdf.js';
   ```

2. 直接看代码

   ```js
   download=()=>{
       // 要导出的dom节点，注意如果使用class控制样式，一定css规则
       const element = document.getElementById('pdf');
       // 导出配置
       const opt = {
           margin: 1,
           filename: '意见调查表',
           image: { type: 'jpeg', quality: 0.98 }, // 导出的图片质量和格式
           html2canvas: { scale: 2, useCORS: true }, // useCORS很重要，解决文档中图片跨域问题
           jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' },
       };
       if (element) {
           html2pdf().set(opt).from(element).save(); // 导出
       }
   }
   ```

### 页面截图

1. 下载并导入html2cannvas

   ```js
   import * as html2canvas from 'html2canvas';
   ```

2. 看函数

   ```js
   scrren=()=>{
       
   	 let screenShot = '';
        let scrrenObject = document.getElementById('page');
       
        (window.html2canvas || html2canvas)(
            scrrenObject,
            {
                useCORS: true,//跨域,不多介绍了
                width: window.screen.availWidth,
                height: document.getElementById('page').offsetHeight,
                x: 0,
                y: 0
         	 }
        ).then((canvas) => {
             screenShot = canvas.toDataURL('image/png');
   		  //这里的screenShot就是我们的图片,是二进制数据
         });
   }
   ```

   
