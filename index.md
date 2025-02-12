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
      /* Force-hide default chat button */
      .embeddedServiceHelpButton {
        display: none !important;
      }
    </style>
  </head>
  <body>
    <!-- Your custom chat button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat</button>

    <script type="text/javascript">
      function initEmbeddedMessaging() {
          try {
              // Set language or any other settings as needed
              embeddedservice_bootstrap.settings.language = 'de';
              // Disable the default chat button
              embeddedservice_bootstrap.settings.displayHelpButton = false;
              // Optionally, try hideChatButtonOnLoad if available:
              // embeddedservice_bootstrap.settings.hideChatButtonOnLoad = true;
              
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

      function launchCustomChat() {
          // Use the API method from InfallibleTechie’s reference article
          embeddedservice_bootstrap.utilAPI.launchChat()
            .then(() => {
              console.log('Inside Launch Chat');
              // Immediately hide the default chat button in case it reappears
              var defaultButton = document.querySelector('.embeddedServiceHelpButton');
              if (defaultButton) {
                defaultButton.style.display = 'none';
              }
            })
            .catch(() => {
              console.log('Inside Launch Chat catch Block');
            })
            .finally(() => {
              console.log('Inside Launch Chat finally Block');
            });
      }
    </script>

    <script type="text/javascript" src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js" onload="initEmbeddedMessaging()"></script>
  </body>
</html>
