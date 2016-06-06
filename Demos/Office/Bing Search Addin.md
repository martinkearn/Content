
# Bing Search add-in
This demo show how to add Bing search capabilities to a standard file > new project > Office task pane

### Pre-reqs
* Visual Studio 2015 with Office tools

## File > New Project
Create a new Task Pane project
* Talk through options as they appear
 
## Add results DIV to `Home.html`
Add this DIV to 'App/Home/Home.html' just beneath the `button`
```
<div id="results">

</div>
```
 
## Declare global service key

In `App/Home/Home.js` right at the top of the file
```
window.thisapp = {};

thisapp.azureServiceKey = "TWFjaGluZUxlYXJuaW5nVGV4dEFuYWx5dGljc1NlcnZpY2VTZW50aW1lbnRBbmFseXNpczp2QUsxMkxkWWNaN21QQnFlQ3VMVFZrQVBIQVZ6MGw1ZkpXbFZpbTVUSEJzPSA=";
```

## Handle get selected data

In `Home.js` in the `getDataFromSelection` function, replace the function (result) contents with this:
```
getSearchResults(result.value);
```

The full function will now look like this
```
    // Reads data from current document selection and displays a notification
    function getDataFromSelection() {
        Office.context.document.getSelectedDataAsync(Office.CoercionType.Text,
            function (result) {
                getSearchResults(result.value);
            }
        );
    }
```

## Implement getSearchResults

In `Home.js`, add this new function
```
function getSearchResults(text) {
    if (text != "") {
        var authorization = "Basic " + thisapp.azureServiceKey;
        var accept = "application/json";
        var apiUrl = "https://api.datamarket.azure.com/Bing/Search/v1/Web?Query=%27" + text +"%27";
        $.support.cors = true;
        $.ajax({
            beforeSend: function (xhr) {
                xhr.setRequestHeader("Authorization", authorization);
                xhr.setRequestHeader("ACCEPT", accept);
            },
            url: apiUrl,
            method: 'GET',
            dataType: 'json',
            complete: function (response) {
                var data = JSON.parse(response.responseText);

                var resultsDiv = document.getElementById('results'); 

                data.d.results.forEach(function(entry) {
                    var resultDiv = document.createElement("div");
                    resultDiv.innerHTML = "<a href=\"" + entry.Url + "\"><span>" + entry.Title + "</span></a><br><span>" + entry.Description + "</span><br><br><hr><br>";
                    resultsDiv.appendChild(resultDiv);
                });
            }
        });
    }
}
```

## Run the app
Rune the app

Add a search query to any cell in excel

Select the cell

Click the 'get data from selection' button

##Snippets
This is the full, final Home.js file
```
/// <reference path="../App.js" />

window.thisapp = {};

thisapp.azureServiceKey = "TWFjaGluZUxlYXJuaW5nVGV4dEFuYWx5dGljc1NlcnZpY2VTZW50aW1lbnRBbmFseXNpczp2QUsxMkxkWWNaN21QQnFlQ3VMVFZrQVBIQVZ6MGw1ZkpXbFZpbTVUSEJzPSA=";

(function () {
    "use strict";

    // The initialize function must be run each time a new page is loaded
    Office.initialize = function (reason) {
        $(document).ready(function () {
            app.initialize();

            $('#get-data-from-selection').click(getDataFromSelection);
        });
    };

    // Reads data from current document selection and displays a notification
    function getDataFromSelection() {
        Office.context.document.getSelectedDataAsync(Office.CoercionType.Text,
            function (result) {
                getSearchResults(result.value);
            }
        );
    }

    function getSearchResults(text) {
        if (text != "") {
            var authorization = "Basic " + thisapp.azureServiceKey;
            var accept = "application/json";
            var apiUrl = "https://api.datamarket.azure.com/Bing/Search/v1/Web?Query=%27" + text + "%27";
            $.support.cors = true;
            $.ajax({
                beforeSend: function (xhr) {
                    xhr.setRequestHeader("Authorization", authorization);
                    xhr.setRequestHeader("ACCEPT", accept);
                },
                url: apiUrl,
                method: 'GET',
                dataType: 'json',
                complete: function (response) {
                    var data = JSON.parse(response.responseText);

                    var resultsDiv = document.getElementById('results');

                    data.d.results.forEach(function (entry) {
                        var resultDiv = document.createElement("div");
                        resultDiv.innerHTML = "<a href=\"" + entry.Url + "\"><span>" + entry.Title + "</span></a><br><span>" + entry.Description + "</span><br><br><hr><br>";
                        resultsDiv.appendChild(resultDiv);
                    });
                }
            });
        }
    }
})();
```