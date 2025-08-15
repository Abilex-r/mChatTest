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
  let buttonReady = false;

  // 2) Click-Listener erst NACH DOM-Load registrieren
  document.addEventListener('DOMContentLoaded', () => {
    const btn = document.getElementById('customChatButton');
    if (!btn) return; // Guard
    btn.addEventListener('click', launchCustomChat);
  });

  function initEmbeddedMessaging() {
    try {
      // Basiskonfig vor init
      window.embeddedservice_bootstrap.settings.language = 'de';
      // WICHTIG: hier NICHT den Button per Setting verstecken.
      // Wir verstecken ihn erst NACH der Erstellung (Event).

      // API bereit
      window.addEventListener('onEmbeddedMessagingReady', () => {
        apiReady = true;
        // Hidden-Prechat ist jetzt sicher
        window.embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({ p_number: '0991000231' });
        console.log('[M4W] Ready');
      });

      // Der interne (Standard-)Button wurde erstellt → jetzt ist Launch sicher
      window.addEventListener('onEmbeddedMessagingButtonCreated', () => {
        buttonReady = true;
        console.log('[M4W] Button created');
        // Jetzt ist es safe, den Standard-Button zu verstecken:
        window.embeddedservice_bootstrap.utilAPI.hideChatButton();
        const btn = document.getElementById('customChatButton');
        if (btn) btn.disabled = false;
      });

      // Init starten
      window.embeddedservice_bootstrap.init(
        '00D5t000000Eo5k',
        'DSAMessaging',
        'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894',
        { scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com' }
      );
    } catch (e) {
      console.error('Error loading Embedded Messaging:', e);
    }
  }

  async function launchCustomChat() {
    console.log('Launching chat...');
    const btn = document.getElementById('customChatButton');

    if (!window.embeddedservice_bootstrap || !apiReady || !buttonReady) {
      console.warn('Messaging nicht bereit (apiReady:', apiReady, ', buttonReady:', buttonReady, ')');
      return;
    }
    if (btn) btn.style.display = 'none';

    try {
      await window.embeddedservice_bootstrap.utilAPI.launchChat();
      console.log('Chat launched successfully');
      window.embeddedservice_bootstrap.utilAPI.hideChatButton(); // doppelt hält besser
    } catch (err) {
      console.error('Error launching chat:', err);
      if (btn) btn.style.display = 'inline-flex';
    } finally {
      console.log('Launch chat complete');
    }
  }
</script>

<!-- 3) Bootstrap NACH dem Button laden, init per onload -->
<script
  src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
  onload="initEmbeddedMessaging()">
</script>
  </body>
</html>
