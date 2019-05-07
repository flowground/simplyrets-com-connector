# ![LOGO](logo.png) SimplyRETS **flow**ground Connector

## Description

A generated **flow**ground connector for the SimplyRETS API (version 1.0.0).

Generated from: https://api.apis.guru/v2/specs/simplyrets.com/1.0.0/swagger.json<br/>
Generated at: 2019-05-07T17:44:03+03:00

## API Description

The SimplyRETS API is an exciting step towards making it easier for
developers and real estate agents to build something awesome with
real estate data!

The documentation below makes live requests to our API using the
trial data. To get set up with the API using live MLS data, you
must have RETS credentials from your MLS, which you can then use to
create an app with SimplyRETS. For more information on that
process, please see our [FAQ](https://simplyrets.com/faq), [Getting
Started](https://simplyrets.com/blog/getting-set-up.html) page, or
[contact us](https://simplyrets.com/\#home-contact).

Below you'll find the API endpoints, query parameters, response bodies,
and other information about using the SimplyRETS API. You can run
queries by clicking the 'Try it Out' button at the bottom of each
section.

### Authentication
The SimplyRETS API uses Basic Authentication. When you create an
app, you'll get a set of API credentials to access your
listings. If you're trying out the test data, you can use
`simplyrets:simplyrets` for connecting to the API.

### Media Types
The SimplyRETS API uses the `Accept` header to allow clients to
control media types (content versions). We maintain backwards
compatibility with API clients by allowing them to specify a
content version. We highly recommend setting and explicity media
type when your application reaches production. Both the structure
and content of our API response bodies is subject to change so we
can add new features while respecting the stability of applications
which have already been developed.

To always use the latest SimplyRETS content version, simply use
`application/json` in your application `Accept` header.

If you want to pin your clients media type to a specific version,
you can use the vendor-specific SimplyRETS media type, e.g.
`application/vnd.simplyrets-v0.1+json"`

To view all valid content-types for making an `OPTIONS`, make a
request to the SimplyRETS api root

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/`

The default media types used in our API responses may change in the
future. If you're building an application and care about the
stability of the API, be sure to request a specific media type in the
Accept header as shown in the examples below.

The wordpress plugin automatically sets the `Accept` header for the
compatible SimplyRETS media types.

### Pagination

To paginate through listings, start your query with these
parameters: 'limit=500&lastId=0'. The 'lastId' is the important
part, you can use any limit up to 500. When you receive the
response from the API with the results, check the 'Link' header for
the 'next' link. That link is pre-built to access the next 'page'
of listings. Alternatively, you can use the last listing's 'mlsId'
from the previous request and use that in the next query. For
example:

First query:

curl -u username:password 'https://api.simplyrets.com/properties?limit=500&lastId=0'

If the 'mlsId' in the last listing of the results is '1234567', then the next query will be:

curl -u username:password 'https://api.simplyrets.com/properties?limit=500&lastId=1234567'

...and so one until you have reached the final page of listings.

There a few pieces of useful information about each request stored
in the HTTP Headers:

- `X-Total-Count` shows you the total amount of listings that match
  your current query.
- `Link` contains pre-built pagination links for accessing the next
'page' of listings that match your query.

### RETS Vendor Compliance

Many RETS vendors have strict requirements for showing disclaimers
with specific information embedded. For example, in many areas it's
required to show the timestamp of the time the listings were
refreshed inside a disclaimer or on a listing page.

The timestamp of the last listing refresh timestamp can be found in
one of two spots:

- The `X-SimplyRETS-LastUpdate` header from `GET /properties` or `GET /properties/{mlsId}`

- Calling the API root `/` or properties api endpoint `/properties`
  with an OPTIONS request

  - `OPTIONS /`

    This request will show the last update timestamp for all RETS
    vendors associated with your application. Look for the
    `updates` list in the JSON response.

  - `OPTIONS /properties`

    Using this request, look for the `lastUpdate` field in the JSON
    response.


## Authorization

Supported authorization schemes:
- Basic Authentication

## Actions

### The SimplyRETS OpenHouses API

> This is the main endpoint for accessing openhouses.

#### Input Parameters
* `type` - _optional_ - Request listings by a specific property type. This
defaults to Residential, and you can only specify one type
in a single query.

    Possible values: residential, rental, multifamily, condominium, commercial, land, farm.
* `listingId` - _optional_ - Request openhouses for a specific `listingId`.

* `cities` - _optional_ - Filter the openhouses returned by a list of valid cities.

The `cities` query parameter is case-insensitive.

The list of `cities` provided by your RETS vendor can be
seen by sending an `OPTIONS` request to the `/properties`
endpoint:

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/openhouses`

* `brokers` - _optional_ - Filter the listings returned by brokerage with a Broker ID.
You can specific multiple broker parameters. Note, the Broker
ID is provided by your MLS.

* `agent` - _optional_ - Filter the listings returned by an agent ID.  Note, the
Agent ID is provided by your MLS.

* `minprice` - _optional_ - Filter listings by a minimum price.

* `startdate` - _optional_ - Scheduled date and time of the open house showing
* `offset` - _optional_ - Increase the offset parameter by the limit to go to the
next "page" of listings. Also take a look at the Link HTTP
Header for pre-built pagination.

*NOTE:* Use the `lastId` parameter for pagination.

* `lastId` - _optional_ - Used as a cursor for pagination.

* `limit` - _optional_ - Set the number of listings to return in the response.
This defaults to 20 listings, and can be a maximum of 500.
To paginate through to the next page of listings, take a
look at the `offset` parameter, or the Link in the HTTP
Header.

* `sort` - _optional_ - Sort the response by a specific field. Values starting
with a minus (-) denote descending order, while the others
are ascending.

    Possible values: listprice, -listprice, listdate, -listdate, beds, -beds, baths, -baths.
* `include` - _optional_ - Include a extra fields which are not in the default
response body
- 'association' includes additional HOA data
- 'agreement' information on the listing agreement
- 'garageSpaces' additional garage data
- 'maintenanceExpense' data on maintenance expenses
- 'parking' additional parking data
- 'pool' includes an additional pool description
- 'taxAnnualAmount' include the annual tax amount
- 'taxYear' include the tax year data
- 'rooms' include parameter will include
   any additional rooms as a list.

Note that your MLS must provide these fields in their RETS
data for them to be available in the API response.

In the future, fields which require an 'include' may become available
by default.


### Single OpenHouse Endpoint

> Use this endpoint for accessing a single OpenHouse.

#### Input Parameters
* `openHouseKey` - _required_ - A unique OpenHouse identification key
* `include` - _optional_ - Include a extra fields which are not in the default
response body
- 'association' includes additional HOA data
- 'agreement' information on the listing agreement
- 'garageSpaces' additional garage data
- 'maintenanceExpense' data on maintenance expenses
- 'parking' additional parking data
- 'pool' includes an additional pool description
- 'taxAnnualAmount' include the annual tax amount
- 'taxYear' include the tax year data
- 'rooms' include parameter will include
   any additional rooms as a list.

Note that your MLS must provide these fields in their RETS
data for them to be available in the API response.

In the future, fields which require an 'include' may
become available by default.


### The SimplyRETS Listings API

> This is the main endpoint for accessing your properties. View<br/>
> all of the available query parameters and make requests below!<br/>
> The API uses Basic Authentication, which most HTTP libraries<br/>
> will handle for you. To use the test data (which is what this<br/>
> pages uses), you can use the api key `simplyrets` and secret<br/>
> `simplyrets`. Note that these test listings are not live MLS<br/>
> listings but the data, query parameters, and response bodies<br/>
> will all work the same.

#### Input Parameters
* `q` - _optional_ - A textual keyword search. This parameter will search  the following
fields, when available:
  - listingId (This does _not_ search the `mlsId` field in the SimplyRETS response body)
  - street number
  - street name
  - mls area (major)
  - city
  - subdivision name
  - postal code

* `status` - _optional_ - Request listings by a specific status. This parameter
defaults to active and you can specify multiple statuses
in a single query.

Listing statuses depend on your MLS's availability. Below is
a brief description of each status with possible synonyms which
may map to your MLS-specific statuses
- *Active*: Active Listing which is still on the market
- *ActiveUnderContract*: An offer has been accepted but the listing is still on market. Synonyms: Accepting Backup Offers, Backup Offer, Active With Accepted. Synonyms: Offer, Backup, Contingent
- *Pending*: An offer has been accepted and the listing is no longer on market. Synonyms: Offer Accepted, Under Contract
- *Hold*: The listing has been withdrawn from the market, but a contract
  still exists between the seller and the listing member. Synonyms: Hold, Hold Do Not Show, Temp Off Market
- *Withdrawn*: The listing has been withdrawn from the market, but a contract
  still exists between the seller and the listing member. Synonyms: Hold, Hold Do Not Show, Temp Off Market
- *Closed*: The purchase agreement has been fulfilled or the lease
  agreement has been executed. Synonyms: Sold, Leased, Rented, Closed Sale
- *Expired*: The listing contract has expired
- *Delete*: The listing contract was never valid or other reason for the contract to be nullified. Synonyms: Kill, Zap
- *Incomplete*: The listing has not yet be completely entered and is not yet
  published in the MLS. Synonyms: Draft, Partially Complted
- *ComingSoon*

    Possible values: Active, Pending, Closed, ActiveUnderContract, Hold, Withdrawn, Expired, Delete, Incomplete, ComingSoon.
* `type` - _optional_ - Request listings by a specific property type. This
defaults to Residential and Rental. You can specify
multiple property types in a single query.

    Possible values: residential, rental, multifamily, condominium, commercial, land, farm.
* `subtype` - _optional_ - Request listings by a specific property sub type.

*NOTE* not all sub type filters are available for all vendors.

    Possible values: apartment, boatslip, singlefamilyresidence, deededparking, cabin, condominium, duplex, manufacturedhome, ownyourown, quadruplex, stockcooperative, townhouse, timeshare, triplex, manufacturedonland.
* `agent` - _optional_ - Filter the listings returned by an agent ID.  Note, the
Agent ID is provided by your MLS.

The co-listing agent is not included in this query parameter.

* `brokers` - _optional_ - Filter the listings returned by brokerage with a Broker
ID. For some MLS areas, this is the ListOfficeId (Listing
Office ID).  You can specific multiple broker
parameters. Note, this query parameter is only available
if a Broker ID is provided by your MLS.

* `minprice` - _optional_ - Filter listings by a minimum price.

* `maxprice` - _optional_ - Filter listings by a maximum price

* `minarea` - _optional_ - Filter listings by a minimum area size in Sq Ft.

* `maxarea` - _optional_ - Filter listings by a maximum area size in Sq Ft.

* `minbaths` - _optional_ - Filter listings by a minimum number of bathrooms.

* `maxbaths` - _optional_ - Filter listings by a maximum number of bathrooms.

* `minbeds` - _optional_ - Filter listings by a minimum number of bedrooms.

* `maxbeds` - _optional_ - Filter listings by a maximum number of bedrooms.

* `maxdom` - _optional_ - Filter listings by a maximum number of days on market.
_Note that your MLS must provide Days on Market data._

* `minyear` - _optional_ - Filter listings by a setting a minimum year built.

* `limit` - _optional_ - Set the number of listings to return in the response.
This defaults to 20 listings, and can be a maximum of 500.
To paginate through to the next page of listings, take a
look at the `offset` parameter, or the Link in the HTTP
Header.

* `offset` - _optional_ - Increase the offset parameter by the limit to go to the
next "page" of listings. Also take a look at the Link HTTP
Header for pre-built pagination.

*NOTE:* Use the `lastId` field to paginate response

*NOTE:* If you're offset is too high, you will receive an
`HTTP 400 offset too high` error message.

* `lastId` - _optional_ - Used as a cursor for pagination. When using `lastId`, the `sort` parameter
will not work.

* `vendor` - _optional_ - Used to specify the vendor (MLS) to search from. This
parameter is required on multi-MLS apps, and you can only
query one vendor at a time. To get your vendor id's make
an OPTIONS request to https://api.simplyrets.com.

`curl -XOPTIONS https://api.simplyrets.com/properties`

* `postalCodes` - _optional_ - Filter the listings returned by postal codes / zip
code. You can specify multiple.

* `features` - _optional_ - Filter the listings by specific interior features.  You
can filter by multiple. For example, to filter trial listings
by multiple features you can use,
Return listings that are within a set of latitude
longitude coordinates. For example,

```
Wet Bar
High Ceiling
```

e.g. `https://simplyrets.com/services?features=Wet%20Bar&features=High%20Ceiling`

The list of `features` provided by your RETS vendor can be
seen by sending an `OPTIONS` request to the `/properties`
endpoint:

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/properties`

* `water` - _optional_ - Query water/waterfront listings only. Specify `true` to
filter waterfront listings.

If you specify `water=true`, all listings with any `waterfront` value
will be queried.

If you specify `water=false`, listings which are **NOT** waterfront
listings will be queried.

If you specify `water=LAKE+NAME` or another valid value contained in
your feed, that value will be searched

* `neighborhoods` - _optional_ - Filter the listings returned by specific neighborhoods and
subdivisions. You can specify multiple `neighborhoods` by
using the query parameter multiple times.

The `neighborhoods` query parameter is case-insensitive.

The list of `neighborhoods` provided by your RETS vendor can be
seen by sending an `OPTIONS` request to the `/properties`
endpoint:

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/properties`

* `cities` - _optional_ - Filter the listings returned by specific cities. You can
specify multiple `cities` query parameters.

The `cities` query parameter is case-insensitive.

The list of `cities` provided by your RETS vendor can be
seen by sending an `OPTIONS` request to the `/properties`
endpoint:

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/openhouses`

* `counties` - _optional_ - Filter the listings returned by specific counties. You can
specify multiple `counties` parameters.

The `counties` query parameter is case-insensitive.

The list of `counties` provided by your RETS vendor can be
seen by sending an `OPTIONS` request to the `/properties`
endpoint:

`curl -XOPTIONS -u simplyrets:simplyrets https://api.simplyrets.com/openhouses`

* `points` - _optional_ - Return listings that are within a set of latitude
longitude coordinates. For example;
```
29.723837,-95.69778
29.938275,-95.69778
29.938275,-95.32974
29.723837,-95.32974
```
Note that some MLS's do not provide latitude and longitude
for their listings, which is required for this parameter
to work. In these cases, SimplyRETS offers a [Geocoding
Addon](https://simplyrets.com/services#geocoding).

Check out our
[blog post](https://simplyrets.com/blog/interactive-map-search.html)
on using the `points` parameter to build a map-based app
in javascript.

* `include` - _optional_ - Include a extra fields which are not in the default
response body
- 'association' includes additional HOA data
- 'agreement' information on the listing agreement
- 'garageSpaces' additional garage data
- 'maintenanceExpense' data on maintenance expenses
- 'parking' additional parking data
- 'pool' includes an additional pool description
- 'taxAnnualAmount' include the annual tax amount
- 'taxYear' include the tax year data
- 'rooms' include parameter will include
   any additional rooms as a list.

Note that your MLS must provide these fields in their RETS
data for them to be available in the API response.

In the future, fields which require an 'include' may become available
by default.

    Possible values: association, agreement, garageSpaces, maintenanceExpense, parking, pool, rooms, taxYear, taxAnnualAmount.
* `sort` - _optional_ - Sort the response by a specific field. Values starting
with a minus (-) denote descending order, while the others
are ascending.

    Possible values: listprice, -listprice, listdate, -listdate, beds, -beds, baths, -baths.
* `count` - _optional_ - When set to `false`, The `X-Total-Count` header will not
be returned

Counting the listings can contribute to slower API calls
due to the extra queries that need to be run to get an
exact count.

Disabling count can increase query speeds.


### Single Listing Endpoint

> Use this endpoint for accessing a single listing. When you<br/>
> make a search to the `/properties` endpoint, each listing in<br/>
> the response will contain a unique `mlsId` field which should<br/>
> be used to request that listing on this route.<br/>
> <br/>
> The `mlsId` field is a unique identifier for a listing which<br/>
> is specific to the SimplyRETS API only.  It is different from<br/>
> the `listingId` field is the public number given to a listing<br/>
> by the MLS and is not used here.

#### Input Parameters
* `mlsId` - _required_ - The `mlsId` field is a unique identifier which is specific
to the SimplyRETS API only.  This field is different from
the `listingId` field (which is the public number given to
a listing by the MLS and is not used here).

* `include` - _optional_ - Include a extra fields which are not in the default
response body
- 'association' includes additional HOA data
- 'agreement' information on the listing agreement
- 'garageSpaces' additional garage data
- 'maintenanceExpense' data on maintenance expenses
- 'parking' additional parking data
- 'pool' includes an additional pool description
- 'rooms' include parameter will include
   any additional rooms as a list.

Note that your MLS must provide these fields in their RETS
data for them to be available with valid data in the API
response. If your MLS does not offer these fields, they will
contain 'null'.

In the future, fields which require an 'include' may become available
by default.

    Possible values: association, agreement, garageSpaces, maintenanceExpense, parking, pool, rooms.

## License

**flow**ground :- Telekom iPaaS / simplyrets-com-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
