여러가지 다 해본 결과 애초에 핸드폰 자체 아이피 주소 또는 핸드폰이 접속되어 있는 와이파이 주소로 서버에 접근이 가능한 건지 부터 다시 알아봐야할 듯 ....   
에뮬레이터는 와이파이 아이피가 가짜라서 연결이 안되는 것으로 확인.   
실제 핸드폰은 LTE, 5G 등 연결 될 때 마다 아이피 주소가 다른게 할당 된다는 것 같음.    
아마 예상으로는 localhost로 우리가 코드로 지정해 준 것의 의미를 생각해보면 와이파이 인터넷 등 접속 될 때의 ip 주소가 아니라 핸드폰 기기 고유의 아이피 주소로 연결하면 가능하지 않을까 싶음...

====================================================
## Get permissions
```
// AndroidManifest.xml permissions

<uses-permission android:name = "android.permission.INTERNET"/>
<uses-permission android:name = "android.permission.ACCESS_NETWORK_STATE"/>
```


## 1. 이더넷의 ip 주소 가져오기
```
/** 
* Get IP address from first non-localhost interface
* @param useIPv4 true=return ipv4, false=return ipv6
* @return address or empty string
*/
public static String getIPAddress(boolean useIPv4){
    try{
        List<NetworkInterface> interfaces = Collections.list(NetworkInterface.getNetworkInterfaces());
        for (NetworkInterface intf : interfaces) {
            List<InetAddress> addrs = Collections.list(intf.getIntAddresses());
            for (InetAddress addr : addrs) {
            if (!addr.isLoopbackAddress()) {
                String sAddr = addr.getHostAddress();
                //boolean isIPv4 = InetAddresssUtils.isIPv4Address(sAddr);
                boolean isIPv4 = sAddr.indexOf(':')<0;
                
                if (useIPv4){
                    if (isIPv4)
                        return sAddr ;
                } else{
                    if (!isIPv4) {
                        int delim = sAddr.indexOf('%'); //drop ip6 zone suffix
                        return delim<0 ? sAddr.toUpperCase() : sAddr.substring(0, delim).toUpperCase();
                        
                    }
                }
            }
        }
        }
    }catch (Exception ignored) {} //for now eat exceptions
    return "";
}
```

## 2. WIFI의 ip 주소 가져오기


```
public static String wifiIpAddress(){
    WifiManager wifiManager = (WifiManager) getApplicationContext().getSystemService(WIFI_SERVICE);
    int ipAddress = wifiManager.getConnectionInfo().getIpAddress();
    
    // Convert little-endian to big-endianif needed
    if (ByteOrder.nativeOrder().equals(ByteOrder.LITTLE_ENIAN)){
        ipAddress = Integer.reverseBytes(ipAddress);
    }
    
    byte [] ipByteArray = BigInteger.valueOf(ipAddress).toByteArray();
    
    String ipAddressString;
    
    try{
        ipAddressString = InetAddress.getByAddress(ipByteArray).getHostAddress();
    } catch (UnknownHostException ex) {
        Log.e("WIFIIP", "Unable to get host address.");
        ipAddressString = null;
    }
    
    return ipAddressString;
}
```

--------------------------------------------------------------------------------

<http://blog.naver.com/PostView.nhn?blogId=yuyyulee&logNo=221527734772&categoryNo=20&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView>
