---
author: Kent Chiu
layout: post
title: "JasperReport 101"
date: 2013-05-12 00:49
comments: true
sharing: true
footer: true
categories: 
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



#### jrxml  report定義檔

Jasper Report 是由副檔名為 `jrxml` 的 xml 檔進行報表範本定義，範本檔的主結構可被切割成數個區，每一區有專屬的tag，每個 tag 有各自專屬的位置及功能，
每個 tag 都不是必要的，可以視需求選用，以下列出 Jasper Report 主結構會用到的 tag

1. `<title>` 			- 主標題，只會出現一次
2. `<pageHeader>` 		- 頁首標題，每頁頁首都會出現
3. `<columnHeader>`		- `<detail>` 的 header，會出現在每個detail區前面
4. `<detail>` 			- 內容區，會重覆的出現，一個 detail 對應到 data source 的一個資料列
5. `<columnFooter>`     - `<detail>` 的 footer，會出現在每個detail區後面(可設定只出現在最後的detail區)
6. `<pageFooter>` 		- 頁尾區，每頁頁尾都會出現，通常來放置頁碼 (可設定只出現在最後一頁)
7. `<summary>`			- 總結區，在報表的最後面，只會出現一次
8. `<background>`		- 設定背景圖片


上面的tag，都需要包含 `<band>` ，才能再放入報表元素(Report Element）

另外還有 `group` tag用來做可用來群組化，每個 group tag 下可有自已的 header 跟 footer


每一區的位置如下圖所示

![2013-05-12-jasperreport-101-001.png][]


比較完整的主結構是像這樣

![2013-05-12-jasperreport-101-002.png][]


上面有提到，每一區必須要定義一個唯一的 `<band>` 後，才能放入其他的報表元件 (Report Element), 報表元件有這些 :

![2013-05-12-jasperreport-101-003.png][]


#### Jasper Report 版的 HELLO WORLD

以下的 hello word 範例，我們不用 designer, 改用全手工的方式打造出最簡單的報表定義檔，定義檔內只有 detail 區， detail 區裡顯示靜態的文字 'Hello World'

> 用 designer 產生的定義檔，會多很多 tags，每個 tag 也會多很多屬性，用手工打造比較乾淨，也比較容易理解


###### xml helloworld.jrxml 定義檔
	<?xml version="1.0" encoding="UTF-8"?>
	<jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd" name="myreport">
	 <detail>
	    <band height="20">
	      <staticText>
	        <reportElement x="180" y="0" width="200" height="20"/>
	        <text><![CDATA[Hello World!]]></text>
	      </staticText>
	    </band>
	  </detail>
	</jasperReport>


我們只用了 detail section, 上面有提到 section 內必須要有 `<band>` tag才能放置其他的 Report Element, 我們這邊用到的 Report Element 為
`<staticText>` 用途為顯示靜態文字， `<staticText>` 使用 `<reportElement>` 做基本屬性設定，然後用 `<text>` 設定文字內容


######  java junit test case for export pdf 
	@Test
	public void exportToPDF_statics_text_only() throws Exception {
		JasperReport jasperReport;
		JasperPrint jasperPrint;
		try {
			URI uri = getClass().getResource("/helloworld.jrxml").toURI();
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



`JasperFillManager.fillReport()` 需要三個參數, 

1. 	jasperReport
	jrxml complied 後的 binary file
2.	paramaters
	控制報表的參數設定值
3.	datasource 為 `JRDataSource` 的 subclass


######  java JasperFillManager.fill() 的 source code

`JasperFillManager.fill()` 的 source code 如下

	/**
	 * Fills the compiled report design supplied as the first parameter and returns
	 * the generated report object.
	 * 
	 * @param jasperReport compiled report design object to use for filling
	 * @param parameters   report parameters map
	 * @param dataSource   data source object
	 * @return generated report object
	 */
	public JasperPrint fill(
		JasperReport jasperReport, 
		Map<String,Object> parameters, 
		JRDataSource dataSource
		) throws JRException
	{
		return JRFiller.fill(jasperReportsContext, jasperReport, parameters, dataSource);
	}

JRDataSource 的 subclass 如下圖，我們這個範例，因為只是單純的顯示靜態文字，不需要任何的資料，所以使用 `JREmptyDataSource` 即可

![2013-05-12-jasperreport-101-004.png][]


#### JRMapCollectionDataSource

這個範例改用 java collection 當作 data source

###### helloword_collection.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd"
		name="myreport">
		<field name="USER_NAME" class="java.lang.String" />

		<detail>
			<band height="30">
				<staticText>
					<reportElement x="10" y="0" width="100" height="20" />
					<text><![CDATA[Hello!]]></text>
				</staticText>
				<textField>
					<reportElement x="80" y="0" width="100" height="20" />
					<textFieldExpression><![CDATA[$F{USER_NAME}]]></textFieldExpression>
				</textField>
			</band>
		</detail>

	</jasperReport>


範本定義檔裡用了兩個 report element 

1.	`<staticText>` : 用來顯示靜態文字
2.	`<textField>`  : 文字欄位，裡面的 `<textFieldExpression>` 可以用使用 Jasper Report 的 Expression
	$F{USER_NAME} 為Jasper Report 的 Expression，是變數名稱的 placeholder，在匯出後， 
	placeholder 會被 data source 裡對應的值所取代，有幾個資料列就會重覆幾次
 

######  java 匯出 PDF 的 test case
	@Test
	public void exportToPDF_data_from_collection() throws Exception {
		JasperReport jasperReport;
		JasperPrint jasperPrint;
		try {
			URI uri = getClass().getResource("/helloword_collection.xml").toURI();
			File input = new File(uri);
			jasperReport = JasperCompileManager.compileReport(input.getAbsolutePath());

			Collection<Map<String, ?>> col = Lists.newArrayList();
			col.add(ImmutableMap.<String, Object>of("USER_NAME", "Kent"));
			col.add(ImmutableMap.<String, Object>of("USER_NAME", "Cindy"));
			JRMapCollectionDataSource ds = new JRMapCollectionDataSource(col );
			jasperPrint = JasperFillManager.fillReport(jasperReport, new HashMap(), ds);
			String output = input.getParent() + "/hello1_collection.pdf";
			System.out.println("output to :" +  output);
			JasperExportManager.exportReportToPdfFile(jasperPrint, output);
		} catch (JRException e) {
			e.printStackTrace();
		}
	}


- 這個範例改用 java collection 來當作資料源，所以要用 `JRMapCollectionDataSource`
- 10 ~ 12 行 放了兩個單位的資料('Kent' & 'Cindy') 進去 data source， 所以，在輸出時，會看到這兩組資料會被套用到 detail section 


輸出的PDF結果如下

![2013-05-12-jasperreport-101-005.png][]



[2013-05-12-jasperreport-101-001.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jasperreport-101-001.png
[2013-05-12-jasperreport-101-002.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jasperreport-101-002.png
[2013-05-12-jasperreport-101-003.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jasperreport-101-003.png
[2013-05-12-jasperreport-101-004.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jasperreport-101-004.png
[2013-05-12-jasperreport-101-005.png]: http://blog.kent-chiu.com/images/2013-05-12/2013-05-12-jasperreport-101-005.png

