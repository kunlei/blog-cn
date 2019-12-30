---
title: Apache POI - Excel 
date: 2019-04-02
authorbox: true
sidebar: false
draft: false
categories:
    - "Java"
tags:
    - "excel"
    - "Java"
---

I recently worked on a project that required me to interact with Microsoft Excel to obtain input data and present optimization results.
It turns out that there is a great package for that purpose and all you need to do is include the following dependencies in the Java project .pom file:
```java
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.17</version>
</dependency>

<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.17</version>
</dependency>
```

There are things I do most frequently in my project:
### Read information from Excel
For this purpose, there are some simple steps to follow:

+ create a *Workbook* object using a *FileInputStream*
+ obtain a reference to the sheet from which you want to read data from
+ create an iterator to loop through the rows
+ retrieve individual cells to obtain needed information

Here is an example code:
```java
try {
    FileInputStream fileInputStream = new FileInputStream(fullFilename);
    Workbook workbook = WorkbookFactory.create(fileInputStream);

    Sheet sheet = workbook.getSheet(sheetName);
    if (sheet == null) {
        System.out.println("There is no sheet named " + sheetName + " in the file!");
        return null;
    }

    Iterator<Row> rowIter = sheet.rowIterator();
    while (rowIter.hasNext()) {
        Row row = rowIter.next();
        Cell c = row.getCell(0);
        String value = c.getStringCellValue();
    }

    fileInputStream.close();
    workbook.close();
} catch (Exception e) {
    e.printStackTrace();
}
```
### Read and write information to Excel
For this purpose, there is a little more work to do.

+ create a *FileInputStream* object
+ create a *Workbook* object binding to the *FileInputStream* object
+ update the workbook object
+ create a *FileOutputStream* object and call workbook.write() to write data into file

```
try {
    FileInputStream fileInputStream = new FileInputStream(fullFilename);
    Workbook workbook = WorkbookFactory.create(fileInputStream);
    fileInputStream.close();

    Sheet sheet = workbook.getSheet(sheetName);
    if (sheet == null) {
        sheet = workbook.createSheet(sheetName);
    } 

    for (Data d : data) {
        Row row = sheet.createRow(0);
        row.createCell(0).setCellValue("Hello World!");
    }

    FileOutputStream fileOutputStream = new FileOutputStream(fullFilename);
    workbook.write(fileOutputStream);
    workbook.close();
    fileOutputStream.close();
} catch (Exception e) {
    e.printStackTrace();
}
```

I also create a helper function to clear the content in a sheet:
```java
public static void clearSheet(Sheet sheet) {
    for (int idx = sheet.getLastRowNum(); idx >= sheet.getFirstRowNum(); idx--) {
        Row row = sheet.getRow(idx);
        if (row != null ) {
            sheet.removeRow(row);
        } else {
            break;
        }
    }
}
```

