<!DOCTYPE html>
<html lang="en">
<head>

</head>
<body>
	<article>
		<h1>Setup Hala</h1>		
	</article>



<article>
	
  <hr />
  ## <strong>Preparing pfSense - RestAPI install</strong><br />
  <hr />
  <br />
  Full documentation:<br />
  <a href="https://github.com/aml-one/pfSense-RestAPI" target="_blank">https://github.com/aml-one/pfSense-RestAPI</a><br />
  <br />
  <br />
  
  Installation (in shell):<br />
			
<pre><code>fetch https://github.com/aml-one/pfSense-RestAPI/raw/main/releases/restapi_latest.tar.xz
tar -xf restapi_latest.tar.xz
cd pfSense-pkg-RestAPI
./install.sh</code></pre>
			
			
<br />
- configure credentials and get the apiKey and secret strings<br />
<br />
	
</article>

<article>
		<p>
			<hr />
			## <strong>SERVER</strong> (Requirement: .net 8 Desktop Runtime) 				
			<br />
			<hr />
			<br />
			First need to setup .NET 8 Desktop Runtime then Hala on the server which will be responsible to request rule changes from pfSense host!
			<br />
			<br />
			<a class="dotnet" href="https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-8.0.0-windows-x64-installer" target="_blank">Download .NET 8 Desktop Runtime</a>
			<br />
			<br />
			- server has to have access to pfSense host. (be able to reach it)<br />
			- install the service:<br /><br />

  <a href="https://hala.aml.one/hala_server.msi" target="_blank">Download Hala Server</a>
  <hr />

  (In default, the installer will open up firewall port 13000 and setup a rule with netsh to start listening on that port)<br />
  (If you choose a different port for the app, then don't forget to open the port on firewall <br />
  and allow for listen on that port with the following cmd command: <span style="color:green">netsh http add urlacl url=http://*:13000/ user=Everyone</span><br />
  You can set different user to tighten security eg.: user=DOMAIN\username)<br /><br />
  <b>Note:</b>Based on time settings on pfSense host,change UTCTime=True or UTCTime=False in order to use UTC or local time<br />
  (If the authentication fails, try to change the UTCTime in Config.ini and restart the service)
  <br />
  <hr /><br />


Edit config file: (<span style="color:rgb(222, 161, 48)">ProgramFiles\AmL\Hala - pfSense Auto Rule Changer\Config.ini</span>) [Requires Administrator rights]<br />
<pre><code><b style="color:green;">[pfSense]</b>
<span style="color:blue">HostIP=</span>&lt;pfSense host IP&gt;
<span style="color:blue">UTCTime=</span>&lt;True|False&gt;

<b style="color:green;">[RestAPI]</b>
<span style="color:blue">Secret=</span>&lt;RestAPI Secret encoded in base64&gt;
<span style="color:blue">APIKey=</span>&lt;RestAPI API key encoded in base64&gt;

<b style="color:green;">[WebServer]</b>
<span style="color:blue">ListeningPort=</span>&lt;Port&gt;
<span style="color:blue">Logging=</span>False
</code></pre>

For base64 encode you can use: <a href="https://www.base64encode.org/" target="_blank">https://www.base64encode.org/</a>
		</p>
	</article>



<article>
  <p>
    <hr />
    ## <strong>CLIENT</strong> (Requirement: .net 8 Desktop Runtime)<br />
    <hr />
    <br />			
    - Install .NET 8 Desktop Runtime first<br />
    <br />
    <a class="dotnet" href="https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-8.0.0-windows-x64-installer" target="_blank">Download .NET 8 Desktop Runtime</a>
    <br />
    <br />
    <a href="https://hala.aml.one/hala_client.msi" target="_blank">Download Hala Client</a>
    <br />
    <br />
    Edit config file: (<span style="color:rgb(222, 161, 48)">ProgramFiles\AmL\Hala - Public IP Change Sender App\Config.ini</span>) [Requires Administrator rights]<br />
<pre><code><b style="color:green;">[WebServer]</b>
<span style="color:blue">Debug=</span>False
<span style="color:blue">PostAddress=</span>&lt;eg: http://IP:PORT/&gt;
<span style="color:blue">User=</span>&lt;Part or exact match of pfSense rule Description&gt;
<br />
<b style="color:green;">[Address]</b>
<span style="color:blue">PublicIP=</span>1.1.1.1
</code></pre>
		<br />
		Leave PublicIP at 1.1.1.1 or setup any fake IP which doesn't match the client's current IP
		</p>
	</article>



<article>
  <p>
    <hr />
    ## <strong>How it works?</strong><br />
    <hr />
    <br />
    Service will check the client's public IP in every two minutes (which will getting during server ping).<br />
    When the current IP is different than the one was stored down on the client (Config.ini), then it will request an IP change from server<br />
    And server will change IP in pfSense <strong>(Firewall / NAT / Port Forward)</strong> and <strong>(Firewall / Rules / WAN)</strong> and apply the new rules
  </p>
</article>


<footer>
  <span> 
    &copy; 2023 by <a href="http://www.aml.one">AmL</a>
  </span>
</footer>
</body>
</html>
