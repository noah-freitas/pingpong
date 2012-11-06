Overview
==
Including the client library `pingpong.js` creates a global object `PingPong`.
Methods are called on the PingPong object and callbacks are set by immediately
invoking the returned object with an object literal as a parameter.  I think
this sounds more confusing than it actually is, see examples below.

PingPong Methods
==
```javascript
PingPong.me(email)({
	loggedIn: userLoggedIn,			// User logged in successfully.
	failed: userLoginFailed			// User login failed.
});
```
The current user's identity.
Client-side, this will be the site user identity.
Server-side, this will be the website identity.


```javascript
PingPong.ping(email)({
	data: // JSON message
})({
	received: messageReceived,		// Message successfully delivered & new message from pinged identity.
	delivered: messageDelivered, 	// Message successfully delivered.
	failed: messageFailed			// Message delivery failed.
});
```
Send a message to an identity.


```javascript
PingPong.pong(email)({
	received: messageReceived,		// Message received.
	failed: pongFailed,				// Pong failed.
	repong: true					// Pong email again after message received.
});
```
Receive a message from an identity.

Example Usage
==
```javascript
$(function (global) {
	var p = global.PingPong;

	// authenticate user
	p.me('jane@example.com')({
		loggedIn: userLoggedIn,
		failed: userLoginFailed
	});

	// authenticate site
	p.me('pingpong.ms@sites.pingpong.ms')({
		loggedIn: siteLoggedIn,
		failed: siteLoginFailed
	});

	// send
	p.ping('noah@pingpong.ms')({
		data: // data
	})({
		received: messageReceived,
		delivered: messageDelivered,
		failed: messageFailed
	});

	// receive from
	p.pong('noah@pingpong.ms')({
		received: messageReceived,
		failed: pongFailed,
		repong: true
	});

	// pingpong style
	global.PingPong({
		me: 'jane@example.com',
		loggedIn: userLoggedIn,
		failed: userLoginFailed
	})({
		pong: 'pingpong.ms@sites.pingpong.ms',
		received: messageReceived,
		failed: pongFailed,
		loop: true
	})({
		ping: 'pingpong.ms@sites.pingpong.ms',
		data: {
			type: login,
			user: global.PingPong.me(),  // Current user's identity.
			time: Date()
		},
		received: messageReceived,
		delivered: messageDelivered,
		failed: messageFailed
	});
}(this));
```
