# Java实现短信的发送

```text
public static JSONObject formPostRequest(String url, Map<String, String> map)
            throws KeyManagementException, NoSuchAlgorithmException, ClientProtocolException, IOException {

        SSLContext sslContext = SSLContexts.custom().build();
        HttpClient httpClient = HttpClients.custom().setSslcontext(sslContext).build();
        HttpPost httpPost = new HttpPost(url);

        List<NameValuePair> list = new ArrayList<NameValuePair>();
        Iterator<Entry<String, String>> iterator = map.entrySet().iterator();
        //map.entrySet()：可以瞬间获得所有map对象
        while (iterator.hasNext()) {
            Entry<String, String> elem = iterator.next();
            list.add(new BasicNameValuePair(elem.getKey(), elem.getValue()));
        }
        if (list.size() > 0) {
            UrlEncodedFormEntity entity = new UrlEncodedFormEntity(list, "UTF-8");
            httpPost.setEntity(entity);
        }
        HttpResponse response = httpClient.execute(httpPost);
        return responseResult(response);
    }

    /**
     * GET 请求
     *
     * @param url
     * @param map
     * @return
     * @throws KeyManagementException
     * @throws NoSuchAlgorithmException
     * @throws ClientProtocolException
     * @throws IOException
     */
    public static JSONObject formGetRequest(String url, Map<String, String> map)
            throws KeyManagementException, NoSuchAlgorithmException, ClientProtocolException, IOException {
        SSLContext sslContext = SSLContexts.custom().build();
        HttpClient httpClient = HttpClients.custom().setSslcontext(sslContext).build();
        Iterator<Entry<String, String>> iterator = map.entrySet().iterator();
        StringBuffer urlwithparams = new StringBuffer();
        urlwithparams.append(url + "?");
        while (iterator.hasNext()) {
            Entry<String, String> elem = iterator.next();
            urlwithparams.append(elem.getKey() + "=" + elem.getValue() + "&");
        }
        urlwithparams.deleteCharAt(urlwithparams.length()-1);
        HttpGet httpGet = new HttpGet(urlwithparams.toString());
        HttpResponse response = httpClient.execute(httpGet);
        return responseResult(response);

    }

    private static JSONObject responseResult(HttpResponse response) throws IOException {
        String result = "";
        if (response != null) {
            HttpEntity resEntity = response.getEntity();
            if (resEntity != null) {
                result = EntityUtils.toString(resEntity, "UTF-8");
            }
        }
        JSONObject jsonObject = JSONObject.fromObject(result);
        return jsonObject;
    }
```

