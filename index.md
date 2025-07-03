<html> Add commentMore actions
<body>
<script type='text/javascript'>
    function initEmbeddedMessaging() {
        try {
            embeddedservice_bootstrap.settings.language = 'de'; // For example, enter 'en' or 'en-US'
            
            
            window.addEventListener("onEmbeddedMessagingReady", () => {            
                console.log( "Inside Prechat API!!" );
                embeddedservice_bootstrap.prechatAPI.setHiddenPrechatFields( { "p_number" : "0453762192" } );
                
            });

            embeddedservice_bootstrap.init(
                '00D0900000AEyYE',
                'DSAMessaging',
                'https://direct-sales.my.site.com/ESWDSAMessaging1736496168770',
                {
                    scrt2URL: 'https://direct-sales.my.salesforce-scrt.com'
                }
            );
        } catch (err) {
            console.error('Error loading Embedded Messaging: ', err);
        }
    };
</script>
<script type='text/javascript' src='https://direct-sales.my.site.com/ESWDSAMessaging1736496168770/assets/js/bootstrap.min.js' onload='initEmbeddedMessaging()'></script>

</body>

</html>
