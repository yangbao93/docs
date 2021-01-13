# Empty 和 Blank的区别（trim 、strip）
isNotEmpty等价为  str != null && str.length>0
isNotBlank 等价为  str != null && str.length>0 && str.trim().length > 0

* 关于isEmpty
	* StringUtils.isEmpty(null) = true
	* StringUtils.isEmpty("") = true
	* StringUtils.isEmpty(" ") = false
	* StringUtils.isEmpty("bob") = false
	* StringUtils.isEmpty(" no ") = false

* 关于isNotEmpty
	* StringUtils.isNotEmpty(null) = false
	* StringUtils.isNotEmpty("") = false
	* StringUtils.isNotEmpty(" ") = true

* 关于isBlank(判断字符是不是为空或长度为0或由空白符构成)
	* StringUtils.isBlank(null) = true
	* StringUtils.isBlank("") = true
	* StringUtils.isBlank(" ") = true
	* StringUtils.isBlank("  ") = true
	* ::StringUtils.isBlank("/t /n /f /r") = true （对于制表符，换行符，换页符，回车符均识别为空白符）::
	* StringUtils.isBlank("/b") = false (/b为单词边界符)
	* StringUtils.isBlank("ad") = false

* 关于trim（去掉字符串两边的控制符[control characters，char <= 32]，如果输入null返回null）
	* StringUtils.trim(null) = null
	* StringUtils.trim("") = ""
	* StringUtils.trim("  ") = ""
	* StringUtils.trim("/t /n /f /r") = ""
	* StringUtils.trim("/tss/b") = "ss"
	* StringUtils.trim(" a aa aaa ") = "a aa aaa"
	* StringUtils.trim(" aa ") = "aa"

* 关于trimToNull
	* StringUtils.trimToNull(null) = null
	* StringUtils.trimToNull("") = null
	* StringUtils.trimToNull(" ") = null

* 关于trimToEmpty
	* StringUtils.trimToEmpty(null) = ""
	* StringUtils.trimToEmpty("") = ""
	* StringUtils.trimToEmpty(" ") = ""

* 关于strip（去掉字符串两段的空白符，输入null返回null）
	* StringUtils.strip(null) = null
	* StringUtils.strip("") = ""
	* StringUtils.strip(" ") = ""
	* StringUtils.strip("/b /t /n /f /r") = "/b"
	* StringUtils.strip("/tss /b") = "ss /b"

* 关于stripToNull
	* StringUtils.stripToNull(null) = null
	* StringUtils.stripToNull("") = null
	* StringUtils.stripToNull(" ") = null

* 关于stripToEmpty
	* StringUtils.stripToEmpty(null) = ""
	* StringUtils.stripToEmpty("") = ""
	* StringUtils.stripToEmpty(" ") = ""