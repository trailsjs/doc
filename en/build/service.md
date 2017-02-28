#### [Docs](../) / [Build](./) / Service

# 2.2. Service

Services are invoked by Controller handlers to perform more complex business logic not directly related to client communication.

## <a href="#sec-annual-reports">SEC Annual Reports</a>

Let's build a simple web service that returns the annual report of a given company as a JSON object. As in the previous section, we'll need a Controller to handle the request/response cycle, but here we'll additionally create a Service that the Controller will invoke to perform some more customized business logic.

#### `yo trails:controller ReportController`
#### `yo trails:service ReportService`

### <a href="#implement-reportcontroller">Implement `ReportController`</a>

```js
// api/controller/ReportController.js

const boom = require('boom')

module.exports = class ReportController extends Controller {

  /**
   * Return the most recent annual report of the given company
   * @param request.params.cik  The Central Index Key of the company
   */
  getLatest (request, reply) {
    const { cik }  = request.params

    if (!/^\d*$/.test(cik)) {
      return reply(boom.badRequest('CIK is not valid'))
    }

    this.services.ReportService.getLatest(cik)
      .then(report => reply(report))
      .catch(err => reply(err))
  }
}
```

### <a href="#implement-reportservice">Implement `ReportService`</a>

`ReportService` will query the [SEC EDGAR](https://www.sec.gov/edgar/searchedgar/cik.htm) service. Their web service returns an RSS feed, which we'll parse into JSON. `getLatest(cik)` will determine the most recent entry in the feed, and return it. Separating out this business logic from the request-handling duties of the Controller is a good practice for separating concerns and organizing code.

For reference, here's a snippet of the RSS feed for General Motors:

```xml
<company-info>
  <addresses>
    <address type="business">
      <city>DETROIT</city>
      <phone>313.556.5000</phone>
      <state>MI</state>
      <street1>300 RENAISSANCE CENTER</street1>
      <zip>48265-3000</zip>
    </address>
  </addresses>
  <assigned-sic>3711</assigned-sic>
  <assigned-sic-desc>MOTOR VEHICLES &amp;amp; PASSENGER CAR BODIES</assigned-sic-desc>
  <cik>0001467858</cik>
  <conformed-name>General Motors Co</conformed-name>
  <fiscal-year-end>1231</fiscal-year-end>
  <state-location>MI</state-location>
</company-info>
```

To store this in our database, we'll want to parse it into JSON, so that it looks something like this:

```json
{
  "companyInfo": {
    "addresses": [
      {
        "type": "business",
        "city": "DETROIT",
        "phone": "313.556.5000",
        "state": "MI",
        "street1": "300 RENAISSANCE CENTER",
        "zip": "48265-3000"
      }
    ],
    "assignedSicDesc": "MOTOR VEHICLES & PASSENGER CAR BODIES",
    "cik": "0001467858",
    "conformedName": "General Motors Co",
    "fiscalYearEnd": "1231"
  }
}
```


```js
// api/services/ReportService.js

const request = require('request-promise')
const qs = require('querystring')

module.exports = class ReportService extends Service {

  /**
   * Query the SEC EDGAR database, retrieve the report, and convert to JSON.
   * @param cik
   */
  getLatest (cik) {
    return this.getEdgarListings(cik)
      .then(list => list.sort(f => (0 - f.filingDate.valueOf())))
      .then(([ filing ]) => filing)
  }

  /**
   * Get list of annual report filings from the SEC EDGAR database for the
   * given company. In SEC lingo, these are called "10-K" reports.
   */
  getEdgarListings (cik) {
    const edgarAtomUrl = 'https://www.sec.gov/cgi-bin/browse-edgar'
    const query = { action: 'getcompany', CIK: cik, type: '10-K', output: 'atom' }

    return request(`${edgarAtomUrl}?${query}`)
      .then(feed => this.parseFeed(feed))
  }

  /**
   * Parse the RSS feed and convert to JSON
   */
  parseFeed (feed) {
    // Lets return something
    return feed.substring(2, 5)
  }
}
```

### <a href="#configure-the-route">Configure the Route</a>

```js
// config/routes.js
module.exports = [
  {
    method: [ 'GET' ],
    path: '/report/{cik}',
    handler: 'ReportController.getLatest'
  }
]
```

### <a href="#example-request-and-response">Example Request and Reponse</a>

- Request: `GET /report/0001467858` ([General Motors](https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0001467858&owner=exclude&count=40))
- Response:
  ```json
  {
    "size": "23 MB",
    "filingDate": "2017-02-07",
    "company": {
      "conformedName": "General Motors Co",
      "state": "MI",
      // ...
    },
    // ...
  }
  ```

- Request: `GET /report/0000928465` ([Amcon Distributing](https://www.sec.gov/cgi-bin/viewer?action=view&cik=928465&accession_number=0001558370-16-009684&xbrl_type=v))
- Response: 
  ```json
  {
    "size": "7 MB",
    "filingDate": "2016-11-08",
    "company": {
      "conformedName": "AMCON DISTRIBUTING CO",
      "state": "NE",
      // ...
    },
    // ...
  }
  ```

- Request: `GET /report/enron`
- Response: 
  ```json
  {
    "statusCode": 400,
    "error": "CIK is not valid",
    "message": ""
  }
  ```

We can get annual reports from the Securities and Exchange Commission in JSON! Very exciting, right? 

### Next: [Policy](policy.md)
