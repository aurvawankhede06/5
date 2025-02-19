<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Restaurant</title>
  <style>
    /* Add your CSS styles here */
    .container { margin: 0 auto; max-width: 960px; }
    .tile { border: 1px solid #ccc; padding: 20px; text-align: center; }
    .row { display: flex; justify-content: space-around; }
  </style>
  <script src="js/jquery.min.js"></script>
  <script src="js/ajax-utils.js"></script>
</head>
<body>
  <div id="main-content">
    <div class="container">
      <div class="row">
        <div class="col-lg-4 col-md-6 col-sm-12">
          <a href="#" id="specialsTile" onclick="$dc.loadMenuItems('{{randomCategoryShortName}}');">
            <div class="tile">
              <h2>Specials</h2>
            </div>
          </a>
        </div>
        <!-- Other tiles for Menu and Map can be added here -->
      </div>
    </div>
  </div>

  <script>
    $(function () {
      $("#navbarToggle").blur(function (event) {
        var screenWidth = window.innerWidth;
        if (screenWidth < 768) {
          $("#collapsable-nav").collapse('hide');
        }
      });

      var categoryShortNames = ["L", "D", "SP", "S", "B"];

      function getRandomCategoryShortName() {
        var randomIndex = Math.floor(Math.random() * categoryShortNames.length);
        return categoryShortNames[randomIndex];
      }

      var originalSpecialsTileContent = $("#specialsTile").html();

      $("#specialsTile").click(function (event) {
        event.preventDefault();
        var randomCategoryShortName = getRandomCategoryShortName();
        var newSpecialsTileContent = originalSpecialsTileContent.replace("{{randomCategoryShortName}}", randomCategoryShortName);
        $("#specialsTile").html(newSpecialsTileContent);
        $("#specialsTile a").click();
      });
    });

    (function (global) {
      var dc = {};
      var homeHtml = "snippets/home-snippet.html";

      var insertHtml = function (selector, html) {
        var targetElem = document.querySelector(selector);
        targetElem.innerHTML = html;
      };

      var showLoading = function (selector) {
        var html = "<div class='text-center'>";
        html += "<img src='images/ajax-loader.gif'></div>";
        insertHtml(selector, html);
      };

      var insertProperty = function (string, propName, propValue) {
        var propToReplace = "{{" + propName + "}}";
        string = string.replace(new RegExp(propToReplace, "g"), propValue);
        return string;
      };

      document.addEventListener("DOMContentLoaded", function (event) {
        showLoading("#main-content");
        $ajaxUtils.sendGetRequest(homeHtml, function (responseText) {
          document.querySelector("#main-content").innerHTML = responseText;
        }, false);
      });

      global.$dc = dc;
    })(window);
  </script>
</body>
</html>
