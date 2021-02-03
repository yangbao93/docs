# byte\[\]数组转outputStream

byte\[\] data = Base64.decodeBase64\(jpgString\); String fileName = UUID.randomUUID\(\).toString\(\) + ".jpg"; response.addHeader\("Content-Disposition", "attachment;filename=" + new String\(fileName.getBytes\("utf-8"\),"iso8859-1"\)\); response.addHeader\("Content-Length", "" + data.length\);

BufferedOutputStream bos = new BufferedOutputStream\(response.getOutputStream\(\)\); response.setContentType\("image/jpeg"\); bos.write\(data\); } catch \(Exception e\) { throwException\("调用接口错误", e\); return; }

