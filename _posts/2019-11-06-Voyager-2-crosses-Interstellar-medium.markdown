Voyager 2 has crossed the Solar system and reached Interstellar medium on November 5th of 2019.

// Sample web code for tracking implementation and testing

// Tracking firing
// PageViewEvent: anchor page
export default Ember.Route.extend({
  // property 
  pageKey: 'MyPageKey',
  // or function
  pageKey: function() { return 'MyPageKey'; },
  ...
});

// PageViewEvent: non anchor page
export default Ember.Component.extend(PageViewComponentMixin, {
  pageKey: 'MyPageKey'
});

// ControlInteractionEvent
<!-- link -->
{{#link-to 'my-page' data-control-name="MyControlName"}}

<!-- button -->
<button {{action 'submit' (tracking control-name="MyOtherControlName")}}>Submit</button>

<!-- component -->
{{#my/component controlName="MyControlName"}}

// Product events
export default Ember.Component.extend(TrackableComponentMixin, {
  onImpression: function(e) {
	  this.get('tracking').fireTrackingPayload('MyProductEvent', impressionTimestamp: e.visibleTime);
  }
});

// Tracking testing
function test1() {
  var trackingSession = generateTrackingSession();

  // normal test code
  visit('/').visit('/');
  click('.like');

  // tracking validation
  trackingSession.assertPageViewEvent('home').occurs(2); // PVE
  trackingSession.assertEvent(function(trackingEvent) {  // product event for clicking like
      return trackingEvent.get('info.topicName') === 'home_action';
    }, 'Custom action event');
  trackingSession.assertInteractionEvent('like').withInteractionType('SHORT_PRESS'); // CIE for clicking like

  trackingSession.close();
}
  					        
