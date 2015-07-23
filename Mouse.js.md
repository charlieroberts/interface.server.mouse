Mouse
=========
The mouse IO tracks mouse movements across a computer screen (regardless of 
whether or not Interface.Server is focused) as well as left and right button
presses. Currently OS X only due to screenres dependency.

    var EventEmitter = require( 'events' ).EventEmitter,
        os           = require( 'os' ),
        sr           = require( 'screenres' ),
        _mouse       = os.platform() === 'darwin' ? require( 'osx-mouse' )() : null,
        _            = require( 'lodash'),

    Mouse = {
      inputs: {},
      outputs:{
        x: { min:0, max:1, value: 0 },
        y: { min:0, max:1, value: 0 },
        leftButton:  { min:0, max:1, value: 0 },
        rightButton: { min:0, max:1, value: 0 },        
      },
      res: sr.get(),
      devices:[],
      getDeviceNames: function() { /* return _.pluck( this.devices, 'product' ) */ },
      init: function() {    
        if( this.onload ) this.onload()
    
        Mouse.devices.push( Mouse )
            
        this.emit( 'new device', 'mouse', Mouse )
          
        _mouse.on( 'move', function( x, y ) {
          Mouse.emit( 'x', x / Mouse.res[0] )
          Mouse.emit( 'y', y / Mouse.res[1] )
          
          // console.log( "MOVE", x / Mouse.res[0], y / Mouse.res[1] )
        })
      }
    }

    Mouse.__proto__ = new EventEmitter()
    Mouse.__proto__.setMaxListeners( 0 )
    
    module.exports = Mouse