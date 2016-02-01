 #Bing Search add-in
 
## For `Home.html`
```
<div id="results">

</div>
```
 
## Declare global service key

In `Home.js` at the top
```
window.thisapp = {};

thisapp.azureServiceKey = "TWFjaGluZUxlYXJuaW5nVGV4dEFuYWx5dGljc1NlcnZpY2VTZW50aW1lbnRBbmFseXNpczp2QUsxMkxkWWNaN21QQnFlQ3VMVFZrQVBIQVZ6MGw1ZkpXbFZpbTVUSEJzPSA=";
```

## Handle get selected data

In `Home.js` in the `getDataFromSelection` function
```
getSearchResults(result.value);
```

## Implement getSearchResults

In `Home.js`, new function
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