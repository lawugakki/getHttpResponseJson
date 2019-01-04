# getHttpResponseJson
## 在调用第三方接口的时候,使用的一个方法, 使用httpclient去发送post请求

public static JSONObject getHttpResponseJson(String url,Map<String,String> param){
        CloseableHttpClient httpclient = null;
        CloseableHttpResponse response = null;
        JSONObject jsonObject = null;
        
        try {
            //请求发起客户端
            httpclient = HttpClients.createDefault();
            //参数集合
            List postParams = new ArrayList();
            //遍历参数并添加到集合
            for(Map.Entry<String, String> entry:param.entrySet()){
                postParams.add(new BasicNameValuePair(entry.getKey(), entry.getValue()));
            }
            //通过post方式访问
            HttpPost post = new HttpPost(url);
            HttpEntity paramEntity = new UrlEncodedFormEntity(postParams,"UTF-8");
            post.setEntity(paramEntity);
            response = httpclient.execute(post);
            HttpEntity valueEntity = response.getEntity();
            String content = EntityUtils.toString(valueEntity);
            jsonObject = JSONObject.fromObject(content);
            
            return jsonObject;
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            if(httpclient != null){
                try {
                    httpclient.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(response != null){
                try {
                    response.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return jsonObject;
        
      
public static void main(String[] args) {
	Map paraMap = new HashMap<>();
	paramMap.put("key1","value1");
	paramMap.put("key2","value2");
	paramMap.put("key3","value3");
	JSONObject jsonObject = getHttpResponseJson("url", paraMap);
	System.out.println(jsonObject);
	}
