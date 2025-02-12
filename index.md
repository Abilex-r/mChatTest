<html>
  <head>
    <style type="text/css">
      /* Force-hide the default Salesforce chat button */
      .embeddedServiceHelpButton {
        display: none !important;
      }
      /* Custom button styling */
      .customChatButton {
        background-color: #00adef;
        border-radius: 50%;
        width: 56px;
        height: 56px;
        border: none;
        cursor: pointer;
        position: fixed;
        bottom: 10px;
        right: 10px;
      }
      .customChatButton:hover {
        background-color: #0098ce;
      }
    </style>
  </head>
  <body>
    <!-- Your custom chat button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
          try {
              // Set language or any other settings
              embeddedservice_bootstrap.settings.language = 'de';

              // Hide the default chat button on load (if supported)
              embeddedservice_bootstrap.settings.hideChatButtonOnLoad = true;
              
              window.addEventListener("onEmbeddedMessagingReady", function() {
                  console.log("Embedded Messaging is ready");
              });
              
              embeddedservice_bootstrap.init(
                  '00D5t000000Eo5k',
                  'DSAMessaging',
                  'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894',
                  {
                      scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com'
                  }
              );
          } catch (err) {
              console.error('Error loading Embedded Messaging: ', err);
          }
      }

      // Workaround: simulate a click on the default (hidden) button
      function launchCustomChat() {
          // Try to find the default button element in the DOM
          var defaultButton = document.querySelector('.embeddedServiceHelpButton');
          if (defaultButton) {
              // Even if hidden, a click event may trigger the chat window to open
              defaultButton.click();
          } else {
              console.error("Default chat button not found");
          }
      }
    </script>

    <!-- Load the Salesforce Messaging bootstrap script -->
    <script type="text/javascript" src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js" onload="initEmbeddedMessaging()"></script>
  </body>
</html>
