<html>
  <head>
    <style type="text/css">
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
      /* Optional CSS override to help keep the default button hidden */
      .embeddedServiceHelpButton {
        display: none !important;
      }
    </style>
  </head>
  <body>
    <!-- Your custom chat button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat with our Agents!</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
        try {
          // Set language and hide the default chat button BEFORE initialization.
          embeddedservice_bootstrap.settings.language = 'de';
          embeddedservice_bootstrap.settings.hideChatButtonOnLoad = true;
          
          window.addEventListener("onEmbeddedMessagingReady", function() {
            console.log("Embedded Messaging is ready");
            // Optionally, force hide the chat button immediately on ready:
            embeddedservice_bootstrap.utilAPI.hideChatButton();
          });
          
          // Initialize the embedded messaging service.
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
        console.log('Launching chat...');
        embeddedservice_bootstrap.utilAPI.launchChat()
          .then(() => {
            console.log('Chat launched successfully');
            // Immediately hide the default button (if it appears after launch)
            embeddedservice_bootstrap.utilAPI.hideChatButton();
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
    <script type="text/javascript"
            src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
            onload="initEmbeddedMessaging()">
    </script>
  </body>
</html>
