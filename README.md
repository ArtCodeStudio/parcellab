# Node.js ParcelLab API

This is a simple (unofficial) wrapper to interface the parcelLab API to [Node.js](https://nodejs.org/), based on [this project](https://bitbucket.org/parcellab/sdk-node) but rewritten in Typescript.

## Background

To use the API, you have to have a set of valid credentials (`user` and `token`) from [parcelLab](https://portal.parcellab.com/).

Any issues can be submitted in the [Git repository's issue tracker](https://github.com/ArtCodeStudio/parcellab-node/issues).

## Install

Preferred way of installation is through [npm](https://www.npmjs.com/package/parcellab).

```
npm install parcellab --save
```

Alternatively, you can clone the Git [repository on GitHub](https://github.com/ArtCodeStudio/parcellab-node).

```
git clone https://github.com/ArtCodeStudio/parcellab-node.git
```

## Usage

You can find an Javascript and Typescript example in [examples](https://github.com/ArtCodeStudio/parcellab-node/tree/main/examples):

```javascript
const { ParcelLabApi } = require('parcellab');
const parcellab = new ParcelLabApi(1, 'ParcelLabApitoken-30characters');

const payloadTracking = {
  courier: 'dhl-germany',
  tracking_number: '1234567890',
  zip_code: '12345',
  destination_country_iso3: 'DEU',
  articles: []
};

try {
  const result = await parcellab.createOrUpdateTracking(payloadTracking);
  console.log(result);
} catch (error) {
  console.error("Error on transfer tracking", error);
}

```

For required and optional properies see the [interfaces](https://github.com/ArtCodeStudio/parcellab-node/tree/main/src/interfaces).

## Dealing with multiple tracking numbers

The module features dealing with multiple tracking numbers embedded in the payload. This allows to use one single call of `createOrUpdateTracking(payload)` for creating multiple trackings for a single order, e.g. when an order from a customer is shipped in several deliveries.

### Multiple deliveries with same courier

If all shipments are done with a single courier, multiple tracking numbers can simply listed with either the delimiter `,` or `|` within the attribute `tracking_number` like so:

```javascript
var payload = {
  courier: 'dhl-germany',
  tracking_number: '1234567890,1234567891,1234567892'
};
```

### Multiple deliveries with multiple couriers

In the more complex case where the deliveries are not performed by the same courier, the tracking numbers can be embedded via `JSON` by using the name of the courier as the key and the associated tracking numbers in an array. Example:

```javascript
var payload = {
  courier: 'XXX', // will be ignored
  tracking_number: {
    'dhl-germany': ['1234567890', '1234567891'],
    'ups': ['1Z1234567']
  }
};
```

## Connect your Store with parcelLab

If you own a Shopify store we already have a ready to use connection for you, just contact us at [hi@artandcode.studio](mailto:hi@artandcode.studio?subject=ParcelLab Shopify Integration request).

## License (ISC)  

~~~~
ISC License

Copyright (c) 2020, Art+Code Studio (https://artandcode.studio)
Copyright (c) 2019, Julian Krenge <julian@parcellab.com> (https://parcellab.com)

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

~~~~