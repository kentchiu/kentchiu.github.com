---
author: Kent Chiu
layout: post
title: "Spring Rest檔案上傳及下載"
date: 2013-03-10 14:27
comments: true
sharing: true
footer: true
categories:
  - spring
  - rest
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



File upload


``` 
	@RequestMapping(value = "/foobar/upload", method = RequestMethod.POST)
	public @ResponseBody
	String upload(@RequestParam("file") MultipartFile file, @RequestParam("fileId") String fileId) {
		logger.info("save import file {} to {}", file.getOriginalFilename(), fileId);

		File resultHome = getWorkingDir();
		File temp = new File(resultHome, fileId);
		try {
			IOUtils.copy(file.getInputStream(), new FileOutputStream(temp));
			return "{success: true}";
		} catch (FileNotFoundException e) {
			logger.error("upload file fail", e);
		} catch (IOException e) {
			logger.error("upload file fail", e);
		}
		return "{success: false}";
	}

```


File Download


``` 
	@RequestMapping(value = "/forbar/export", method = RequestMethod.GET)
	public HttpEntity<byte[]> excelExcel() throws IOException {
		File file = new File("myexcel.xls");
		byte[] body = FileUtils.readFileToByteArray(file);
		HttpHeaders header = new HttpHeaders();
		header.setContentType(new MediaType("application", "xls"));
		header.set("Content-Disposition", "attachment; filename=" + "foobar.xls");
		header.setContentLength(body.length);
		return new HttpEntity<byte[]>(body, header);
	}

```


