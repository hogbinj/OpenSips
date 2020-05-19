Freeswitch Gateway <-> Opensips local  -  Opensips Public    <->   Microsoft
   10.0.0.4:5080   <-> 10.0.0.5:5060  - 137.117.136.143:5091     [MS GATEWAYS]   
   
Opensips-cp

[MS GATEWAYS]
ms1	0	sip.pstnhub.microsoft.com	  0	 	Always	tls:137.117.136.143:5091		
ms2	0	sip2.pstnhub.microsoft.com	0	 	Always	tls:137.117.136.143:5091			
ms3	0	sip3.pstnhub.microsoft.com	0	 	Always	tls:137.117.136.143:5091

[ROUTE]
1	1	<prefix>	0	1	ms1, ms2, ms3	 	Send to MS Teams
<prefix> the start of the number to route

[PERMISSIONS]
 0	 13.80.245.144	   32	 5080	 udp	 	 pbx.ip-sentinel.com
 0	 10.0.0.4	         32	 5080	 udp	 	 pbx.ip-sentinel.com
 1	 137.117.136.143	 32	 5091	 tls	 	 sbc.ip-sentinel.com
 1	 10.0.0.5	         32	 5091	 tls	 	 sbc.ip-sentinel.com	
	

