#  relayr for Web Developers - Introduction

We don't want to teach you how to create a web app. We are quite positive that you know your way around web development tools. Below you will find code samples which will allow you to use PubNub, our [data distribution service](https://developer.relayr.io/documents/PubNub/Reference) in your web app. 

## Subscribing an App to a Device Channel

The first step in receiving data from a device is to subscribe to its specific channel. You can read more about the underlying architecture in our [Cloud Platform Introduction](https://developer.relayr.io/documents/Welcome/Platform).


The API call which establishes the connection between a specific app and a device, and allows the app to start receiving data from it is the following:

####[POST] http://api.relayr.io/devices/{deviceId}/apps/{appId}

Where `appId` and `deviceId` are the IDs of the application and the device to be paired.

#### Implementation Example (JavaScript):

	
    $.ajax({
	    url: "https://api.relayr.io/apps/"+appId+ "/devices/"+ deviceId,
	    type: 'post',
	    headers: {
	      "Authorization": 'Bearer '+ token 
	    },
	    dataType: 'application/json',
	    complete: function (credentials) {
      		var pubnub = PUBNUB.init({
        		ssl: true,
        		subscribe_key : credentials.subscribeKey,
        		cipher_key: credentials.cipherKey,
        		auth_key: credentials.authKey
      		});
		}
	});


The call results in the delivery of the channel and the specific PubNub credentials for the channel (`cipherKey` and `authKey`).

#### For Example:

	{
	    "authKey": "8a292336-23d2-4f16-b7e1-1c17be11a532",
	    "cipherKey": "2d4589958e2f51ad6c590a6b0a4382c50c6713eef3d578210d2d135214b05eed",
	    "channel": "v1:6854bb1c-64ed-4d46-99b4-6cf28b72ad1d:data"
	}


## Example: Receiving Data from PubNub and Displaying it (JavaScript)

The example below is a simple step by step walk through the process of subscription to a device channel, data retrieval and display. 

**NOTE**: The code below assumes that the OnBoarding process has been initiated. The onBoarding process is currently only available via the mobile apps. Visit our [OnBoarding Page](https://developer.relayr.io/dashboard/onboarding) to download the onBoarding app. 

To register your App please visit the <a href="https://developer.relayr.io/dashboard/apps/myApps" target="_blank"> Apps section on the Developer Dashboard </a>


#### 1. Make sure to download the PubNub <a href="http://www.pubnub.com/docs/javascript/javascript-sdk.html" target="_blank"> SDK/Library </a> and incorporate it in your project.

----------


#### 2. You will need three variables in order to connect your app to your onboarded device:

		var appId= "YOUR APP ID"; 
		var deviceId= "YOUR DEVICE ID";
		var token ="YOUR TOKEN";



 

- To get your `appId` visit the <a href="https://developer.relayr.io/dashboard/apps/myApps" target="_blank">Apps section in the Developer Dashboard </a>

 

- To get your `deviceId`, once you've onboarded your WunderBar,  go to the <a href="https://developer.relayr.io/dashboard/devices">Devices section on the Developer Dashboard </a> and click on the small 'settings' icon of any of your sensors to get its Id.

 

- To get your `token` please follow the instructions on the <a href="https://developer.relayr.io/documents/WEB/OAuthToken">Getting your OAuth token page </a>

----------


#### 3. Initiate the API call which subscribes the App to the Device and returns PubNub credentials.

		
    $.ajax({
	    url: "https://api.relayr.io/apps/"+appId+ "/devices/"+ deviceId,
	    type: 'post',
	    headers: {
	      "Authorization": 'Bearer '+ token 
	    },
	    dataType: 'application/json',
	    success: function (credentials) {
	 
      var pubnub = PUBNUB.init({
        ssl: true,
        subscribe_key : credentials.subscribeKey,
        cipher_key: credentials.cipherKey,
        auth_key: credentials.authKey
 
      });


----------


#### 4. Subscribe to the channel by passing the channel ID and output the data.

 		pubnub.subscribe({
        channel : credentials.channel,
        
        message : function(data){
          //Output the realtime data coming from the device
          document.write(data)
	        }
	      }); 
	    }
	


----------

Now that you understand how to incorporate receiving data via the relayr platform. You can go ahead and create your first web project. Check out our simple and quick <a href="https://github.com/relayr/android-demo-apps/tree/master/thermometer" target="_blank">Don't Touch This sample project</a> to get inspired.