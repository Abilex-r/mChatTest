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
  let uiEventsBound = false;

  // Sichtbarkeit deiner Bubble steuern
  function showCustomBubble() {
    const btn = document.getElementById('customChatButton');
    if (btn) {
      btn.style.display = 'flex';
      btn.disabled = false;
    }
  }
  function hideCustomBubble() {
    const btn = document.getElementById('customChatButton');
    if (btn) btn.style.display = 'none';
  }

  // UI-Events nur einmal binden
  function bindMessagingUiEventsOnce() {
    if (uiEventsBound) return;
    uiEventsBound = true;

    // 1) Empfohlene Events (wenn verfügbar)
    window.addEventListener('onEmbeddedMessagingWindowExpanded', hideCustomBubble);   // Chat-Fenster aufgeklappt → Bubble weg
    window.addEventListener('onEmbeddedMessagingWindowMinimized', showCustomBubble); // Chat-Fenster minimiert → Bubble zurück
    window.addEventListener('onEmbeddedMessagingSessionEnded', showCustomBubble);    // Session endet → Bubble zurück

    // 2) Fallback per MutationObserver (falls obige Events in deiner Version nicht feuern)
    const obs = new MutationObserver(() => {
      // Suche nach dem Messaging-UI-IFrame (robuster generischer Selector)
      const iframe =
        document.querySelector('iframe.embeddedMessagingFrame') ||
        document.querySelector('iframe[src*="embeddedservice"]') ||
        document.querySelector('iframe[src*="embedded-messaging"]');

      if (!iframe) {
        // Kein Iframe sichtbar → Bubble zeigen
        showCustomBubble();
        return;
      }

      const cs = getComputedStyle(iframe);
      const visible = cs.display !== 'none' && cs.visibility !== 'hidden' && iframe.offsetWidth > 0 && iframe.offsetHeight > 0;
      if (visible) hideCustomBubble(); else showCustomBubble();
    });

    obs.observe(document.body, { childList: true, subtree: true, attributes: true });
  }

  function initEmbeddedMessaging() {
    try {
      // Sprache setzen (weitere Settings hier, aber NICHT hideChatButtonOnLoad)
      window.embeddedservice_bootstrap.settings.language = 'de';

      // API "bereit"
      window.addEventListener('onEmbeddedMessagingReady', () => {
        apiReady = true;
        // Hidden Prechat erst jetzt
        window.embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({ p_number: '0991000231' });
        console.log('[M4W] Ready');
      });

      // Standard-Button erzeugt → jetzt sicher verstecken und Custom-Bubble aktivieren
      window.addEventListener('onEmbeddedMessagingButtonCreated', () => {
        buttonReady = true;
        console.log('[M4W] Button created');

        // Standard-Bubble ausblenden (im DOM lassen!)
        window.embeddedservice_bootstrap.utilAPI.hideChatButton();

        // Deine Bubble aktivieren
        const btn = document.getElementById('customChatButton');
        if (btn) btn.disabled = false;

        // UI-Events/Fallback binden
        bindMessagingUiEventsOnce();
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
    if (!window.embeddedservice_bootstrap || !apiReady || !buttonReady) {
      console.warn('Messaging noch nicht bereit (apiReady:', apiReady, ', buttonReady:', buttonReady, ')');
      return;
    }

    // Beim Öffnen: deine Bubble ausblenden
    hideCustomBubble();

    try {
      await window.embeddedservice_bootstrap.utilAPI.launchChat();
      console.log('Chat launched successfully');

      // Doppelt absichern, dass Standard-Bubble unsichtbar bleibt
      window.embeddedservice_bootstrap.utilAPI.hideChatButton();
    } catch (err) {
      console.error('Error launching chat:', err);
      // Bei Fehler: Bubble wieder einblenden
      showCustomBubble();
    } finally {
      console.log('Launch chat complete');
    }
  }

  // Click erst registrieren, wenn DOM geladen
  document.addEventListener('DOMContentLoaded', () => {
    const btn = document.getElementById('customChatButton');
    if (btn) btn.addEventListener('click', launchCustomChat);
  });
</script>

<!-- Bootstrap im BODY laden, dann init -->
<script
  src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
  onload="initEmbeddedMessaging()">
</script>
  </body>
</html>
