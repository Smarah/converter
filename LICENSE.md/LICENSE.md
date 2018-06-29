
import idb from 'idb';
function openDatabase() {
  // If the browser doesn't support service worker,
  // we don't care about having a database
  if (!navigator.serviceWorker) {
    return Promise.resolve();
  }
   return idb.open('converter', 1, function(upgradeDb) {
    var store = upgradeDb.createObjectStore('converter', {
      keyPath: 'id'
    });
    store.createIndex('by-date', 'time');
  });
}
export default function IndexController(container) {
  this._container = container;
  this._dbPromise = openDatabase();
  this._registerServiceWorker();
  var indexController = this;
  this._showCachedMessages().then(function() {
    indexController._openSocket();
  });
}
IndexController.prototype._registerServiceWorker = function() {
  if (!navigator.serviceWorker) return;
  navigator.serviceWorker.register('/sw.js');
    };
document.getElementById('convertButton').addEventListener('click', convertCurrency);
function convertCurrency( ){

    let fromCurrency = document.getElementById("currency-from").value;
    let toCurrency = document.getElementById("currency-to").value;
    let amount = document.getElementById("amount").value;
    let result = document.getElementById("result-display");

    let query = `${fromCurrency}_${toCurrency}`;
    let url = 'http://free.currencyconverterapi.com/' + query + '&compact=y';

    fetch(url)
        .then((response) => {
            response.JSON();
        }).then((data) => {
            console.log(data);
            let value = data[query].val;
            console.log(value);

            if (value !== undefined) {
                let total = parseFloat(value) * parseFloat(amount);
                result.innerHTML = total;
                console.log(result);
            } else {
                var err = new Error("Value not found for " + query);
                console.log(err);
            }

        });
}


