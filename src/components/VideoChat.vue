<template>
  <div class="hello">
    <div id="otEmbedContainer" style="width:800px; height:640px"></div>
    <div id="videos">
      <div id="subscriber"></div>
      <div id="publisher" ></div>
    </div>
    <div id="ui-controls">
      <select v-model="selected">
        <option>Room 1</option>
        <option>Room 2</option>
        <option>Room 3</option>
      </select>
    </div>
  </div>
</template>

<script>
const OT = require('@opentok/client');

var apiKey = "47304684";
var sessionId = "2_MX40NzMwNDY4NH5-MTYyODk3ODE5ODA4N345UzBGYmdEU01UZGVieEV2Z1cvbmpXaVJ-fg";
var token = "T1==cGFydG5lcl9pZD00NzMwNDY4NCZzaWc9ZTE5N2U3ZTVjOGJmNjU4OGExZDk0ZjczNTRmMWE3NDBmOWQwNGRlYTpzZXNzaW9uX2lkPTJfTVg0ME56TXdORFk0Tkg1LU1UWXlPRGszT0RFNU9EQTROMzQ1VXpCR1ltZEVVMDFVWkdWaWVFVjJaMWN2Ym1wWGFWSi1mZyZjcmVhdGVfdGltZT0xNjI4OTc4MjMxJm5vbmNlPTAuMzM0NjcwMTU2NDI0OTkyMyZyb2xlPXB1Ymxpc2hlciZleHBpcmVfdGltZT0xNjMxNTcwMjI2JmluaXRpYWxfbGF5b3V0X2NsYXNzX2xpc3Q9";

function handleError(error) {
  if (error) {
    alert(error.message);
  }
}

function initializeSession() {
  var session = OT.initSession(apiKey, sessionId);

  session.on('streamCreated', function(event) {
    session.subscribe(event.stream, 'subscriber', {
      insertMode: 'replace',
      width: '100%',
      height: '100%'
    }, handleError);
  });

  var publisher = OT.initPublisher('publisher', {
    insertMode: 'append',
    width: '100%',
    height: '100%'
  }, handleError);

  session.connect(token, function(error) {
    if (error) {
      handleError(error);
    } else {
      session.publish(publisher, handleError);
    }
  });
}

export default {
  name: 'VideoChat',
  selected: 'Room 1',
  mounted() {
      var SERVER_BASE_URL = "https://hidden-bastion-09895.herokuapp.com";
      fetch(SERVER_BASE_URL + '/session').then(function(res) {
        return res.json()
      }).then(function(res) {
        apiKey = res.apiKey;
        sessionId = res.sessionId;
        token = res.token;
        initializeSession();
      }).catch(handleError);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body, html {
    background-color: gray;
    height: 100%;
}

#videos {
    position: relative;
    width: 100%;
    height: 100%;
    margin-left: auto;
    margin-right: auto;
}

#subscriber {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 10;
}

#publisher {
    position: absolute;
    width: 360px;
    height: 240px;
    bottom: 10px;
    left: 10px;
    z-index: 100;
    border: 3px solid white;
    border-radius: 3px;
}
</style>
