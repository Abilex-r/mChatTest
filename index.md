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

      /* Hide the standard Salesforce chat button */
      .embeddedServiceHelpButton {
        display: none !important;
      }
    </style>
  </head>
  <body>
    <!-- Custom Chat Button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat with us</button>

    <script>
  let apiReady = false;

  // Wartet, bis der interne Button im DOM ist UND tatsächlich im Dokument hängt
  function waitForInternalButton(timeoutMs = 10000) {
    const selector = '.embeddedServiceHelpButton';
    return new Promise((resolve, reject) => {
      const btn = document.querySelector(selector);
      if (btn && document.body.contains(btn)) return resolve(btn);

      const obs = new MutationObserver(() => {
        const el = document.querySelector(selector);
        if (el && document.body.contains(el)) {
          obs.disconnect();
          resolve(el);
        }
      });
      obs.observe(document.documentElement || document.body, {
        childList: true, subtree: true
      });

      setTimeout(() => {
        obs.disconnect();
        reject(new Error('Timeout: internal messaging button not found in DOM'));
      }, timeoutMs);
    });
  }

  function initEmbeddedMessaging() {
    try {
      // Sprache usw. vor init:
      window.embeddedservice_bootstrap.settings.language = 'de';
      // WICHTIG: NICHT hideChatButtonOnLoad = true setzen,
      // der Standard-Button muss im DOM erzeugt werden.

      // API bereit:
      window.addEventListener('onEmbeddedMessagingReady', () => {
        apiReady = true;
        // Hidden Prechat erst hier:
        window.embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({
          p_number: '0991000231'
        });

        // Sobald der interne Button existiert, Custom-Button aktivieren
        waitForInternalButton()
          .then(() => document.getElementById('customChatButton').disabled = false)
          .catch(err => console.warn(err.message));
      });

      // Init starten
      window.embeddedservice_bootstrap.init(
        '00D5t000000Eo5k',
        'DSAMessaging',
        'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894',
        { scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com' }
      );
    } catch (e) {
      console.error('Error loading Embedded Messaging: ', e);
    }
  }

  async function launchCustomChat() {
    console.log('Launching chat...');
    try {
      if (!window.embeddedservice_bootstrap || !apiReady) {
        console.warn('API noch nicht bereit.');
        return;
      }

      // Sicherstellen, dass der interne Button JETZT im DOM hängt
      await waitForInternalButton();

      // Erst jetzt launchen
      document.getElementById('customChatButton').style.display = 'none';
      await window.embeddedservice_bootstrap.utilAPI.launchChat();
      console.log('Chat launched successfully');

    } catch (err) {
      console.error('Error launching chat:', err);
      document.getElementById('customChatButton').style.display = 'flex';

      // Fallback: zur Not den Standard-Button direkt klicken (falls sichtbar gemacht)
      // const nativeBtn = document.querySelector('.embeddedServiceHelpButton button');
      // if (nativeBtn) nativeBtn.click();

    } finally {
      console.log('Launch chat complete');
    }
  }

  document.getElementById('customChatButton').addEventListener('click', launchCustomChat);
</script>

<!-- Bootstrap IM BODY laden, dann init auf onload -->
<script
  src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
  onload="initEmbeddedMessaging()">
</script>
  </body>
</html>
