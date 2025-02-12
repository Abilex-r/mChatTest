<html>
  <head>
    <style type="text/css">
      /* Custom chat button styling */
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
      /* Force-hide the default button if it appears */
      .embeddedServiceHelpButton {
        display: none !important;
      }
    </style>
  </head>
  <body>
    <!-- Custom Chat Button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat with our Agents!</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
          try {
              // Configure the embedded service settings
              embeddedservice_bootstrap.settings.language = 'de';
              // Disable the default chat button
              embeddedservice_bootstrap.settings.displayHelpButton = false;
              // Optionally, also try hideChatButtonOnLoad if supported
              // embeddedservice_bootstrap.settings.hideChatButtonOnLoad = true;
              
              window.addEventListener("onEmbeddedMessagingReady", function() {
                  console.log("Embedded Messaging is ready");
              });
              
              embeddedservice_bootstrap.init(
                  '00D5t000000Eo5k',
                  'DSAMessaging',
                  'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894',
                  { scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com' }
              );
          } catch (err) {
              console.error('Error loading Embedded Messaging: ', err);
          }
      }

      function launchCustomChat() {
          // Launch the chat using the utilAPI method provided by Salesforce Messaging
          embeddedservice_bootstrap.utilAPI.launchChat()
            .then(() => {
              console.log('Chat launched successfully');
              // No need to search for the default button here
            })
            .catch(() => {
              console.log('Error launching chat');
            })
            .finally(() => {
              console.log('Launch chat complete');
            });
      }
    </script>

    <!-- Load the Salesforce Messaging bootstrap script -->
    <script type="text/javascript" src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js" onload="initEmbeddedMessaging()"></script>
  </body>
</html>
