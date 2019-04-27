# Please-Help-With-Alexa-Code-
says The skill's audio content URL(s) are not accessible. Please ensure the audio content URL(s) provided in the skill are accessible at all times. even though has been acive here and on win amp had to use a diffrent link since the other link needed to be https witch was done using bit.ly
for some reason eventhough works in beta link here
https://skills-store.amazon.com/deeplink/tvt/4557b7d7fbd3992a8e061d49cc8b67c526b46bd7136f86b5847ee29d6cdb7c82249706bd419e7b09fc60e173a62fef7f01e8063d3ba79ed1c3d2e9128db266183ad1513979f278165ecd6031e8c24cb02ec18e2135669fb8190a5ae3c5ec94b9a3c650ce44158723ea165da6f9632d59
'use strict';

var Alexa = require('alexa-sdk');

var streamInfo = {
  title: 'Acidv1p3r Radio',
  subtitle: 'Acidv1p3r Radio live stream',
  cardContent: "Enjoy",
  url: 'https://bit.ly/2PzzT2w'
};

exports.handler = (event, context, callback) => {
  var alexa = Alexa.handler(event, context, callback);

  alexa.registerHandlers(
    handlers,
    audioEventHandlers
  );

  alexa.execute();
};

var handlers = {
  'LaunchRequest': function() {
    this.emit('PlayStream');
  },
  'PlayStream': function() {
    this.response.speak('Now playing AcidV1p3r Radio').audioPlayerPlay('REPLACE_ALL', streamInfo.url, streamInfo.url, null, 0);
    this.emit(':responseReady');
  },
  'AMAZON.HelpIntent': function() {
    this.response.speak('This skill plays live stream of AcidV1p3r Radio')
    this.emit(':responseReady');
  },
  'SessionEndedRequest': function() {
    this.response.speak('Thanks for listening to AcidV1p3r Radio')
    this.emit(':responseReady');
  },
  'ExceptionEncountered': function() {
    console.log("\n---------- ERROR ----------");
    console.log("\n" + JSON.stringify(this.event.request, null, 2));
    this.callback(null, null)
  },
  'Unhandled': function() {
    this.response.speak('Sorry. Something went wrong.');
    this.emit(':responseReady');
  },
  'AMAZON.NextIntent': function() {
    this.response.speak('This skill does not support skipping.');
    this.emit(':responseReady');
  },
  'AMAZON.PreviousIntent': function() {
    this.response.speak('This skill does not support skipping.');
    this.emit(':responseReady');
  },
  'AMAZON.PauseIntent': function() {
    this.emit('AMAZON.StopIntent');
  },
  'AMAZON.CancelIntent': function() {
    this.emit('AMAZON.StopIntent');
  },
  'AMAZON.StopIntent': function() {
    this.response.speak('Okay. I\'ve stopped the stream.').audioPlayerStop();
    this.emit(':responseReady');
  },
  'AMAZON.ResumeIntent': function() {
    this.emit('PlayStream');
  },
  'AMAZON.LoopOnIntent': function() {
    this.emit('AMAZON.StartOverIntent');
  },
  'AMAZON.LoopOffIntent': function() {
    this.emit('AMAZON.StartOverIntent');
  },
  'AMAZON.ShuffleOnIntent': function() {
    this.emit('AMAZON.StartOverIntent');
  },
  'AMAZON.ShuffleOffIntent': function() {
    this.emit('AMAZON.StartOverIntent');
  },
  'AMAZON.StartOverIntent': function() {
    this.response.speak('Sorry. I can\'t do that yet.');
    this.emit(':responseReady');
  },
  'PlayCommandIssued': function() {

    if (this.event.request.type === 'IntentRequest' || this.event.request.type === 'LaunchRequest') {
      var cardTitle = streamInfo.subtitle;
      var cardContent = streamInfo.cardContent;
      this.response.cardRenderer(cardTitle, cardContent);
    }

    this.response.speak('Enjoy.').audioPlayerPlay('REPLACE_ALL', streamInfo.url, streamInfo.url, null, 0);
    this.emit(':responseReady');
  },
  'PauseCommandIssued': function() {
    this.emit('AMAZON.StopIntent');
  }
}

var audioEventHandlers = {
  'PlaybackStarted': function() {
    this.emit(':responseReady');
  },
  'PlaybackFinished': function() {
    this.emit(':responseReady');
  },
  'PlaybackStopped': function() {
    this.emit(':responseReady');
  },
  'PlaybackNearlyFinished': function() {
    this.response.audioPlayerPlay('REPLACE_ALL', streamInfo.url, streamInfo.url, null, 0);
    this.emit(':responseReady');
  },
  'PlaybackFailed': function() {
    this.response.audioPlayerClearQueue('CLEAR_ENQUEUED');
    this.emit(':responseReady');
  }
}
