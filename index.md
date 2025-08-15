<html>
  <head>
    <style type="text/css">
      /* Custom chat button styling */
      .customChatButton {
        background-color: rgb(0, 173, 239);
        border-radius: 50%;
        width: 80px; height: 80px;
        border: none; cursor: pointer;
        position: fixed; bottom: 20px; right: 20px;
        z-index: 2147483647;
        display: flex; flex-direction: column;
        align-items: center; justify-content: center;
        text-align: center; color: #fff;
        font-size: 13px; font-weight: bold;
        white-space: nowrap; padding: 8px; line-height: 1.2;
        box-shadow: 2px 2px 10px rgba(0,0,0,.3);
      }
      .customChatButton:hover { background-color: rgb(0,150,210); }
      
    </style>
  </head>
  <body>
    <!-- Custom Chat Button -->
    <button class="customChatButton" onclick="launchCustomChat()">Chat with us</button>

    <script>
      let apiReady = false;
      let buttonReady = false;
      let uiEventsBound = false;

      function showCustomBubble() {
        const btn = document.getElementById('customChatButton');
        if (btn) { btn.style.display = 'flex'; btn.disabled = false; }
      }
      function hideCustomBubble() {
        const btn = document.getElementById('customChatButton');
        if (btn) btn.style.display = 'none';
      }

      function bindMessagingUiEventsOnce() {
        if (uiEventsBound) return;
        uiEventsBound = true;

        // Chat geöffnet → Bubble aus; minimiert/Session beendet → Bubble an
        window.addEventListener('onEmbeddedMessagingWindowExpanded', hideCustomBubble);
        window.addEventListener('onEmbeddedMessagingWindowMinimized', showCustomBubble);
        window.addEventListener('onEmbeddedMessagingSessionEnded', showCustomBubble);

        // Fallback: sichtbares/unsichtbares Chat-Iframe beobachten
        const obs = new MutationObserver(() => {
          const iframe =
            document.querySelector('iframe.embeddedMessagingFrame') ||
            document.querySelector('iframe[src*="embeddedservice"]') ||
            document.querySelector('iframe[src*="embedded-messaging"]');

          if (!iframe) { showCustomBubble(); return; }
          const cs = getComputedStyle(iframe);
          const visible = cs.display !== 'none' && cs.visibility !== 'hidden' &&
                          iframe.offsetWidth > 0 && iframe.offsetHeight > 0;
          if (visible) hideCustomBubble(); else showCustomBubble();
        });
        obs.observe(document.body, { childList: true, subtree: true, attributes: true });
      }

      function initEmbeddedMessaging() {
        try {
          window.embeddedservice_bootstrap.settings.language = 'de';

          window.addEventListener('onEmbeddedMessagingReady', () => {
            apiReady = true;
            window.embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({ p_number: '0991000231' });
            console.log('[M4W] Ready');
          });

          window.addEventListener('onEmbeddedMessagingButtonCreated', () => {
            buttonReady = true;
            console.log('[M4W] Button created');

            // Standard-Bubble jetzt (wo sie existiert) sauber verstecken
            window.embeddedservice_bootstrap.utilAPI.hideChatButton();

            // Custom-Bubble aktivieren
            const btn = document.getElementById('customChatButton');
            if (btn) btn.disabled = false;

            bindMessagingUiEventsOnce();
          });

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
        hideCustomBubble(); // eigene Bubble sofort ausblenden

        try {
          await window.embeddedservice_bootstrap.utilAPI.launchChat();
          console.log('Chat launched successfully');
          // Sicherheitshalber die Standard-Bubble nochmal verstecken
          window.embeddedservice_bootstrap.utilAPI.hideChatButton();
        } catch (err) {
          console.error('Error launching chat:', err);
          showCustomBubble();
        } finally {
          console.log('Launch chat complete');
        }
      }

      // Click erst nach DOM-Load registrieren
      document.addEventListener('DOMContentLoaded', () => {
        const btn = document.getElementById('customChatButton');
        if (btn) btn.addEventListener('click', launchCustomChat);
      });
    </script>

    <!-- Bootstrap laden, danach init -->
    <script
      src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
      onload="initEmbeddedMessaging()">
    </script>
  </body>
</html>
