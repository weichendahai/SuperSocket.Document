# New Features and Breaking Changes

> __Keywords__: SuperSocket 1.6, New Features, Breaking Changes, textEncoding, defaultCulture, Process Isolation, SuperSocket.Agent.exe


## The new configuration attribute "textEncoding"
In the __SuperSocket__ before 1.6, when you send a text message via session object, the default encoding to convert the text message to binary data which can be sent over socket is UTF8.
You can change it by assigning a new encoding to the session's Charset property.

Now in __SuperSocket__ 1.6, you can define the default text encoding in the configuration.
The new attribute "textEncoding" has been added into the server configuration node:

	<superSocket>
		<servers>
		  <server name="TelnetServer"
			  textEncoding="UTF8"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2020">
		  </server>
		</servers>
	</superSocket>

## The new configuration attribute "defaultCulture"
This new added feature is only for the .Net framework 4.5 and above. It allows you to set default culture for all the threads, no matter how the threads are created, by programming or come from thread pool.

The new configuration attribute "defaultCulture" can be added in the root node or the server node, so you can configure the default culture for all the server instances or for each server instance separately:

	<superSocket defaultCulture="en-US">
		<servers>
		  <server name="TelnetServerA"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2020">
		  </server>
		  <server name="TelnetServerB"
			  defaultCulture="zn-CN"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2021">
		  </server>
		  <server name="TelnetServerC"
			  defaultCulture="zn-TW"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2021">
		  </server>
		</servers>
	</superSocket>

## Process level isolation
In __SuperSocket__ 1.5, we added AppDomain level isolation, you can run your multiple appserver instances in the their own AppDomain. This feature provides higher level security and resource isolation and bring more benefits to the users who run SuperSocket as multiple hosting service.

In __SuperSocket__ 1.6, we introduce much higher level isolation "Process". If you configure the SuperSocket in this kind isolation, you appserver instances will run in the separated processes. The configuration looks like that:

	<superSocket isolation="Process">
		<servers>
		  <server name="TelnetServerA"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2020">
		  </server>
		  <server name="TelnetServerB"
			  serverType="YourAppServer, YourAssembly"
			  ip="Any" port="2021">
		  </server>
		</servers>
	</superSocket>

Beyond the configuration, you need to include an executable assembly "SuperSocket.Agent.exe" into your project output, which is provided by SuperSocket.

After you start your SuperSocket, you will find more processes of SuperSocket:

    SuperSocket.SocketService.exe
    SuperSocket.Agent.exe
	SuperSocket.Agent.exe


## Changes about the performance data collecting
The API collecting performance data has been changed, two virtual method have been changed:

    protected virtual void UpdateServerSummary(ServerSummary serverSummary);

    protected virtual void OnServerSummaryCollected(NodeSummary nodeSummary, ServerSummary serverSummary)


To:

	protected virtual void UpdateServerStatus(StatusInfoCollection serverStatus)

	protected virtual void OnServerStatusCollected(StatusInfoCollection nodeStatus, StatusInfoCollection serverStatus)


The classes __ServerSummary__ and __NodeSummary__ have been removed. Now you should use the class __StatusInfoCollection__.