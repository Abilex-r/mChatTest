<html>
  <head>
    <style type="text/css">
      /* Custom chat button styling */
      .customChatButton {
        background-color: rgb(0, 173, 239); /* Use Case 1: Required Bubble Color */
        border-radius: 50%;
        width: 70px;
        height: 70px;
        border: none;
        cursor: pointer;
        position: fixed;
        bottom: 20px; /* Adjusted for better placement */
        right: 20px;
        z-index: 2147483647; /* Use Case 2: Ensures it's always on top */
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        font-size: 14px;
        font-weight: bold;
        text-align: center;
        white-space: nowrap; /* Prevents text breaking */
        box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.3);
      }
      .customChatButton:hover {
        background-color: rgb(0, 150, 210); /* Slightly darker on hover */
      }
    </style>
  </head>
  <body>
    <!-- Custom Chat Button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat with us</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
        try {
          embeddedservice_bootstrap.settings.language = 'de';
          embeddedservice_bootstrap.settings.hideChatButtonOnLoad = true;
          
          window.addEventListener("onEmbeddedMessagingReady", function() {
            console.log("Embedded Messaging is ready");
            embeddedservice_bootstrap.utilAPI.hideChatButton();
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
        console.log('Launching chat...');
        embeddedservice_bootstrap.utilAPI.launchChat()
          .then(() => {
            console.log('Chat launched successfully');
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

    <script type="text/javascript"
            src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
            onload="initEmbeddedMessaging()">
    </script>
  </body>
</html>
