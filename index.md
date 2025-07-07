<html> Add commentMore actions
<body>
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

<script type='text/javascript' src='https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js' onload='initEmbeddedMessaging()'></script>

</body>

</html>
