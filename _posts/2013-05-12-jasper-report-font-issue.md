---
author: Kent Chiu
layout: post
title: "Jasper Report, IReport 匯出成中文PDF"
date: 2013-05-12 12:22
comments: true
sharing: true
footer: true
tags:
- jasterreport
- ireport
- pdf
- excel 
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------


jasper report 匯出成 PDF 時， 中文字會無法正常匯出， 需做額外的處理，才能正確的輸出中文。 
中文會有問題主要的原因是缺少中文字體的關係，所以只要滿足了這個條件， PDF 就能顯示中文。 

以下步驟為設定 iReport Designer (5.x) 中文字型的方式

#### 工具 ﹣> 選項

![2013-05-12-jesper-report-font-issue-000.png][]

#### 安裝字型

![2013-05-12-jesper-report-font-issue-001.png][]

字形檔為 true type (.ttf)格式，本範例是繁體中文的標楷體 (kaiu.ttf)

![2013-05-12-jesper-report-font-issue-002.png][]

> 可以在windows的字型檔目錄下取得字型檔，但我在測試時無法直接在windows/font的目錄選得字型檔，
> 須把字型檔copy到其他目錄時，iReport 的 *Select True Type Font* dialog 才看得到字型檔。


#### 設定字型檔的細節

![2013-05-12-jesper-report-font-issue-005.png][]

選進來後的Family Name(字型檔名) 預設是 "標楷體"，我把它改名成 *kaiu* (這麼做的原因只是不想讓設定檔出現一堆中文字型名稱，當然，
你也可以保持預設的名稱 "標楷體")，PDF details的設定要特別注意，
PDF ENcodeing要設成 *Identity-H (Unicode with horizontal writer)*, 然後 *Enbed this font in PDF document*要勾選


到這邊，基本上就算設定好了，之後就是要在範本檔 (jrxml) 裡指定中文字時，要使用剛設定出來的這個字型即可

#### 在範本檔設定使用中文字型

文字內容的`font name`要設定為剛剛新安裝的字型檔名稱

![2013-05-12-jesper-report-font-issue-007.png][]

做preivew後就會產出 pdf 檔，裡面的中文就會正常顯示了


### 用程式匯出成pdf檔時中文的問題

上述的步驟，只是讓iReport Designer可以正確的匯出有中文字的 pdf，但如果是要用程式做 pdf 匯出的動作，需要把字型檔 export 成 jar 格式的 extension ，
並丟到 class path 底下，用程式做匯出時，中文字才會正常的顯示。

匯出的功能是在原來安裝字型檔的功能畫面上，裡面有一個 `Export as extension` 的按鍵，執行後設定 export 的副檔名為 *.jar* 即可，ex : kaiu.jar

匯出後，將該 jar 檔丟到 class path下即可；如果執行匯出時，發生字型檔找不到的異常，應該是字型檔的 jar 檔沒正確的放在 class path 下

![2013-05-12-jesper-report-font-issue-006.png][]



######  jrxml 範本定義檔 hello.jrxml


	<?xml version="1.0" encoding="UTF-8"?>
	<jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd" name="myreport">

	 <detail>
	    <band height="20">
	      <staticText>
	        <reportElement x="180" y="0" width="200" height="20"/>
	        <textElement>
				<font fontName="kaiu" isPdfEmbedded="true"/>
			</textElement>
	        <text><![CDATA[Hello!!! World! 中文字測試]]></text>
	      </staticText>
	    </band>
	  </detail>
	</jasperReport>


`<font fontName="kaiu" isPdfEmbedded="true"/>` : fontName 必須設定安裝時設定的名稱


###### 測試程式

	@Test
	public void exportToPDF_statics_text_only() throws Exception {
		JasperReport jasperReport;
		JasperPrint jasperPrint;
		try {
			URI uri = getClass().getResource("/hello.jrxml").toURI();
			File input = new File(uri);
			jasperReport = JasperCompileManager.compileReport(input.getAbsolutePath());
			jasperPrint = JasperFillManager.fillReport(jasperReport, new HashMap(), new JREmptyDataSource());
			String output = input.getParent() + "/hello1.pdf";
			System.out.println("output to :" +  output);
			JasperExportManager.exportReportToPdfFile(jasperPrint, output);
		} catch (JRException e) {
			e.printStackTrace();
		}
	}

執行 test case後，可以看 output 的目錄看到匯出的 PDF 檔中文是否有正常顯示

[2013-05-12-jesper-report-font-issue-000.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-000.png
[2013-05-12-jesper-report-font-issue-001.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-001.png
[2013-05-12-jesper-report-font-issue-002.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-002.png
[2013-05-12-jesper-report-font-issue-005.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-005.png
[2013-05-12-jesper-report-font-issue-007.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-007.png
[2013-05-12-jesper-report-font-issue-006.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jesper-report-font-issue-006.png

