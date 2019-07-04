Import this repository into your Discourse forum via the Themes menu.  

Edit the following JavaScript to your liking and then add it to the </head> section of the "Edit CSS/HTML" area.

```
<div id="foreground"></div>

<script>
    var links = [
        ["Discord Server", "https://discord.gg/EEg47Dw", "fa-microphone"],
        ["Facebook", "http://facebook.com/timefuser", "fa-facebook"],
        ["Twitter", "http://twitter.com/timefuser", "fa-twitter"]
    ];

    function checkForChanges()
    {
        var newItems = $("div.main-link > div.top-row > a.title, .topic-list-main-link a.title, .topic-list .main-link a.title, .latest-topic-list-item .main-link a.title").filter(function() {
            return !$(this).hasClass("topic-highlight");
        });
        
        newItems.each(function() {
            var sibling = $(this).prev("div .topic-statuses");
            
            if (sibling && sibling.hasClass("topic-statuses")) {
                $(this).addClass("topic-highlight");
                $(this).siblings(".topic-statuses").each(function() {
                   $(this).children("a.topic-status").hide();
                });
            }
        });
        
        setTimeout(checkForChanges, 100);
    }
    
    function insertTopbar()
    {
        var navigation = $(".list-controls");
        
        if (navigation && navigation.length > 0)
        {
            var linksTemplate = "";
            
            links.forEach(function(link) {
                if (link[2]) {
                    linksTemplate += "<li class='topbar ember-view'><a href='" + link[1] + "'><i class='fa " + link[2] + "' aria-hidden='true' style='margin-right:8px;'></i>" + link[0] + "</a></li>";
                } else {
                    linksTemplate += "<li class='topbar ember-view'><a href='" + link[1] + "'>" + link[0] + "</a></li>";
                }
            });
            
            var topBar = $(`
            <div class='list-controls'>
                <div class='container'>
                    <ul id="topbar" class="nav nav-pills ember-view">
                    ` + linksTemplate + `
                    </ul>
                </div>
            </div>
            `);
            
            navigation.addClass("normal-navigation");
            
            navigation.before(topBar);
        }
    }
    
    $(insertTopbar);
    $(checkForChanges);
</script>
```