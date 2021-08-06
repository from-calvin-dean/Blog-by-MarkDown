---
title: PHP Excel
date: 2021-08-06 18:50:36
tags:
---

1. ### 导入

   1. composer安装phpexcel

      ```
      composer require phpoffice/phpexcel
      ```

   2. 使用头

      ```php
      use PHPExcel\PHPExcel_IOFactory;
      ```

   3. 封装导入函数

      ```php
      function read_excel($filePath)
      {
          $arr =  explode('.',$filePath);
          //获取文件后缀
          $suffix = $arr[count($arr)-1];
          //判断哪种类型
          if($suffix=="xlsx"){
              $reader = \PHPExcel_IOFactory::createReader('Excel2007');
          }else{
              $reader = PHPExcel_IOFactory::createReader('Excel5');
          }
          //载入excel文件
          $excel = $reader->load("$filePath",$encode = 'utf-8');
          //读取第一张表
          $sheet = $excel->getSheet(0);
          //获取总行数
          $row_num = $sheet->getHighestRow();
          //获取总列数
          $col_num = $sheet->getHighestColumn();
          $data = []; //数组形式获取表格数据
          for ($i = 1; $i <= $row_num; $i ++) {
              $data[$i][]  = $sheet->getCell("A".$i)->getValue();
              $data[$i][]  = $sheet->getCell("B".$i)->getValue();
              $data[$i][]  = $sheet->getCell("C".$i)->getValue();
        		//这边对应的是excel的每一列
          }
          return $data;
      }
      ```

   ### 导出

   1. 省略直接看封装的函数

      ```php
      /**
       * excel表格导出
       * @param string $fileName 文件名称
       * @param array $headArr 表头名称
       * @param array $data 要导出的数据
       * @author static7
       */
      function orderExcelExport($fileName = '', $headArr = [], $data = [])
      {
      
          $fileName .= "_" . date("Y_m_d", time()) . ".xls";
      
          $objPHPExcel = new \PHPExcel();
      
          $objPHPExcel->getProperties();
      
          $key = ord("A"); // 设置表头
      
          foreach ($headArr as $v) {
      
              $colum = chr($key);
      
              $objPHPExcel->setActiveSheetIndex(0)->setCellValue($colum . '1', $v);
      
              $objPHPExcel->setActiveSheetIndex(0)->setCellValue($colum . '1', $v);
      
              $key += 1;
      
          }
      
          $column = 2;
      
          $objActSheet = $objPHPExcel->getActiveSheet();
      
          foreach ($data as $key => $rows) { // 行写入
      
              $span = ord("A");
      
              foreach ($rows as $keyName => $value) { // 列写入
      
                  $objActSheet->setCellValue(chr($span) . $column, $value);
      
                  $span++;
      
              }
      
              $column++;
      
          }
      
          $fileName = iconv("utf-8", "gb2312", $fileName); // 重命名表
      
          $objPHPExcel->setActiveSheetIndex(0); // 设置活动单指数到第一个表,所以Excel打开这是第一个表
      
          header('Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet');
      
          header('Content-Disposition: attachment;filename="'.$fileName.'.xlsx"');
      
          header('Cache-Control: max-age=0');
      
          $objWriter = \PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel2007');
      
          $objWriter->save('php://output'); // 文件通过浏览器下载
      
          exit();
      
      }
      ```

   2. 调用方法如下

      ```php
      public function orderExport(){
          $dataArr = Db::name('order')->get();
          $cellName = [
                      'ID',
                      '单号',
                      '姓名',
                      '手机号',
                      '商品',
                      '价格',
                      '个人所得',
                      '状态'
                  ];
          orderExcelExport('订单表', $cellName,$dataArr);
      }
      ```

      
