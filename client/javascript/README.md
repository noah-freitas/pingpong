Example Usage:
==

    $(function (global) {
    	var p = global.PingPong;
    	
    	// authenticate user
    	p.me('jane@example.com')({
    		loggedIn: userLoggedIn,
    		failed: userLoginFailed
    	});
    	
    	// authenticate site
    	p.me('pingpong.ms@sites.pingpong.ms')({
    		loggedIn: siteLoggedaIn,
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
    		loop: true
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
    			user: PingPong.loggedIn(),
    			time: Date()
    		},
    		received: messageReceived,
    		delivered: messageDelivered,
    		failed: messageFailed
    	})
    }(this));
