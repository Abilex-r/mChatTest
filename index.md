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
  let chatOpen = false;

  function getCustomBtn() {
    return document.getElementById('customChatButton') || document.querySelector('.customChatButton');
  }

  function showCustomBubble() {
    if (chatOpen) return; // nie zeigen, wenn Chat offen
    const btn = getCustomBtn();
    if (btn) {
      btn.style.setProperty('display', 'flex', 'important');
      btn.disabled = false;
    }
  }

  function hideCustomBubble() {
    const btn = getCustomBtn();
    if (btn) btn.style.setProperty('display', 'none', 'important');
  }

  function bindMessagingUiEventsOnce() {
    if (uiEventsBound) return;
    uiEventsBound = true;

    // Chat aufgeklappt → Bubble aus
    window.addEventListener('onEmbeddedMessagingWindowExpanded', () => {
      chatOpen = true;
      hideCustomBubble();
    });

    // Chat minimiert/Session Ende → Bubble an
    window.addEventListener('onEmbeddedMessagingWindowMinimized', () => {
      chatOpen = false;
      showCustomBubble();
    });
    window.addEventListener('onEmbeddedMessagingSessionEnded', () => {
      chatOpen = false;
      showCustomBubble();
    });

    // Fallback: nur wenn keine Events feuern
    const obs = new MutationObserver(() => {
      if (chatOpen) return; // Events haben Vorrang
      const iframe =
        document.querySelector('iframe.embeddedMessagingFrame') ||
        document.querySelector('iframe[src*="embeddedservice"]') ||
        document.querySelector('iframe[src*="embedded-messaging"]');

      if (!iframe) { showCustomBubble(); return; }
      const cs = getComputedStyle(iframe);
      const visible = cs.display !== 'none' && cs.visibility !== 'hidden' &&
                      iframe.offsetWidth > 0 && iframe.offsetHeight > 0;
      if (visible) { chatOpen = true; hideCustomBubble(); }
      else { chatOpen = false; showCustomBubble(); }
    });
    obs.observe(document.body, { childList: true, subtree: true, attributes: true });
  }

  function initEmbeddedMessaging() {
    try {
      embeddedservice_bootstrap.settings.language = 'de';

      window.addEventListener('onEmbeddedMessagingReady', () => {
        apiReady = true;
        embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields({ p_number: '0991000231' });
        console.log('[M4W] Ready');
      });

      window.addEventListener('onEmbeddedMessagingButtonCreated', () => {
        buttonReady = true;
        console.log('[M4W] Button created');
        embeddedservice_bootstrap.utilAPI.hideChatButton(); // Standardbubble aus
        const btn = getCustomBtn(); if (btn) btn.disabled = false;
        bindMessagingUiEventsOnce();
      });

      embeddedservice_bootstrap.init(
        '00D5t000000Eo5k',
        'DSAMessaging',
        'https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894',
        { scrt2URL: 'https://dsa--uat.sandbox.my.salesforce-scrt.com' }
      );
    } catch (e) { console.error('Error loading Embedded Messaging:', e); }
  }

  async function launchCustomChat() {
    console.log('Launching chat...');
    if (!window.embeddedservice_bootstrap || !apiReady || !buttonReady) {
      console.warn('Messaging nicht bereit (apiReady:', apiReady, ', buttonReady:', buttonReady, ')');
      return;
    }
    chatOpen = true;           // ab jetzt als offen behandeln
    hideCustomBubble();        // hart verstecken

    try {
      await embeddedservice_bootstrap.utilAPI.launchChat();
      console.log('Chat launched successfully');
      embeddedservice_bootstrap.utilAPI.hideChatButton();
    } catch (err) {
      console.error('Error launching chat:', err);
      chatOpen = false;
      showCustomBubble();
    } finally {
      console.log('Launch chat complete');
    }
  }

  document.addEventListener('DOMContentLoaded', () => {
    const btn = getCustomBtn();
    if (btn) btn.addEventListener('click', launchCustomChat);
  });
</script>

<script
  src="https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js"
  onload="initEmbeddedMessaging()">
</script>
  </body>
</html>
