<html>
  <head>
    <style type="text/css">
      /* Custom chat button styling */
      .customChatButton {
        background-color: rgb(0, 173, 239); /* Required Bubble Color */
        border-radius: 50%;
        width: 80px;
        height: 80px;
        border: none;
        cursor: pointer;
        position: fixed;
        bottom: 20px;
        right: 20px;
        z-index: 2147483647; /* Ensure it's always on top */
        display: flex;
        flex-direction: column; /* Aligns text properly */
        align-items: center;
        justify-content: center;
        text-align: center;
        color: white;
        font-size: 13px;
        font-weight: bold;
        white-space: nowrap; /* Prevents text breaking */
        padding: 8px;
        line-height: 1.2;
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
        
        // Hide the custom chat button when chat opens
        document.querySelector('.customChatButton').style.display = 'none';

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

      // Event Listener: Show the button again when chat closes
      window.addEventListener("onEmbeddedMessagingSessionEnded", function() {
        console.log("Chat session ended, showing button again.");
        document.querySelector('.customChatButton').style.display = 'flex';
      });

    </script>

    <!-- Load the Salesforce Messaging bootstrap script -->
    <script type="text/javascript"
            src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
            onload="initEmbeddedMessaging()">
    </script>
  </body>
</html>
