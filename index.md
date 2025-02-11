<html> 
<body>
  <style type='text/css'>

              .embeddedServiceHelpButton .helpButton .uiButton {

                background-color: #00adef;

                border-radius: 50%;

                min-width: 5em;

               margin: 0 15px 15px 0;

            }

              .embeddedServiceHelpButton .helpButton .uiButton:focus {

                outline: 1px solid #00adef;

                border-radius: 50%;

                min-width: 5em;

            }

              .embeddedServiceHelpButton .helpButton {

                margin: 0 15px 25px 0;

            }

              .embeddedServiceHelpButton .helpButton .uiButton .embeddedServiceIcon {

                margin: auto;

            }

              .embeddedServiceHelpButton .embeddedServiceIcon::before {

               font-size: 2.5em;

            }

              .embeddedServiceHelpButton .helpButton .uiButton .helpButtonLabel {

                display: none;

            }

             .embeddedServiceLiveAgentStateChatAvatar.isLightningOutContext .agentIconColor0 {

                background-color: #84bd00;

            }

</style>


<script type='text/javascript'>
    function initEmbeddedMessaging() {
        try {
            embeddedservice_bootstrap.settings.language = 'de'; // For example, enter 'en' or 'en-US'
            embeddedservice_bootstrap.settings.displayHelpButton = false;

            window.addEventListener("onEmbeddedMessagingReady", () => {            
                console.log( "Inside Prechat API!!" );
                embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields( { "p_number" : "0453762192" } );
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
    };
</script>
<script type='text/javascript' src='https://dsa--uat.sandbox.my.site.com/ESWDSAMessaging1721207835894/assets/js/bootstrap.min.js' onload='initEmbeddedMessaging()'></script>


</body>

</html>
