```javascript
//pm.environment.set("count", 100);

  

function sendRequest() {

    pm.sendRequest({

        url: 'http://localhost:9090/questionnaire',

        method: 'POST',

        headers: {

        'x-session': 'a34ad1d6-3838-4ab2-94ac-415cf02cd7ec',

        'x-api-key': 'bst.W0jW18ArJhHH078Hueh9CxUmiPerBk2xvN5bsB74sJ',

        'Content-Type': 'application/json'

  },

        body: {

            mode: 'raw',

            raw: JSON.stringify({ "transactionId": "6001",

    "questionAns": "Y",

    "questionId": "3",

    "comments": "test",

    "isPartialDispute": false,

    "disputeAmount": "104" })

        }

    }, function (err, res) {

       console.log(err)

    });

}

  

for (let i = 0; i < 2; i++) {

    sendRequest();

}

  

const postRequest = {

  url: 'http://localhost:9090/questionnaire',

  method: 'POST',

 headers: {

        'x-session': 'a34ad1d6-3838-4ab2-94ac-415cf02cd7ec',

        'x-api-key': 'bst.W0jW18ArJhHH078Hueh9CxUmiPerBk2xvN5bsB74sJ',

        'Content-Type': 'application/json'

  },

   body: {

            mode: 'raw',

            raw: JSON.stringify({ "transactionId": "6001",

    "questionAns": "Y",

    "questionId": "3",

    "comments": "test",

    "isPartialDispute": false,

    "disputeAmount": "104" })

        }

};

pm.sendRequest(postRequest, (error, response) => {

  console.log(error ? error : response.json());

});

```