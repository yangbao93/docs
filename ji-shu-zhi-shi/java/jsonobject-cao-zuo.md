# JSONObject操作

JSONObject操作： 近日使用到json对象的传递，关于值得获取和存放，以及判断是否为空等操作。 整理如下，

public void addUser\(HttpServletRequest request, HttpServletResponse response, @RequestBody JSONObject jsonObject\) { jsonObject.getString\("key"\);值得获取 jsonObject.put\("key","value"\);放入key\_value jsonObject.has\("key"\);判断key是否存在 }

