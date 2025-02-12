<html>
  <body>
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
        /* Additional custom styles can go here */
      }
      .customChatButton:hover {
        background-color: #0098ce;
      }
    </style>

    <!-- Your custom chat button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
          try {
              // Set your language or other settings as needed
              embeddedservice_bootstrap.settings.language = 'de';

              // Disable the default chat button
              embeddedservice_bootstrap.settings.displayHelpButton = false;

              window.addEventListener("onEmbeddedMessagingReady", function() {
                  console.log("Embedded Messaging is ready");
                  // You can also set pre-chat fields or additional configurations here
              });
              
              // Initialize the embedded messaging service
              embeddedservice_bootstrap.init(
                  '00D5t000000Eo5k',  // Your Salesforce Org Id
                  'DSAMessaging',     // Your deployment name
                  'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894', // Your deployment URL
                  {
                      scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com'
                  }
              );
          } catch (err) {
              console.error('Error loading Embedded Messaging: ', err);
          }
      }

      // Function that will launch the chat using Salesforce's provided method
      function launchCustomChat() {
          // This method should open the chat window
          embeddedservice_bootstrap.showHelp();
      }
    </script>

    <!-- Load the Salesforce messaging bootstrap script -->
    <script type="text/javascript" 
            src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js" 
            onload="initEmbeddedMessaging()">
    </script>
  </body>
</html>
