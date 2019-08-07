# tap-recharge

This is a [Singer](https://singer.io) tap that produces JSON-formatted data
following the [Singer
spec](https://github.com/singer-io/getting-started/blob/master/SPEC.md).

This tap:

- Pulls raw data from the [ReCharge Payments API](https://developer.rechargepayments.com/)
- Extracts the following resources:
  - [Addresses](https://developer.rechargepayments.com/#list-addresses)
  - [Charges](https://developer.rechargepayments.com/#list-charges)
  - [Collections](https://developer.rechargepayments.com/#list-collections-alpha)
  - [Customers](https://developer.rechargepayments.com/#list-customers)
  - [Discounts](https://developer.rechargepayments.com/#list-discounts)
  - [Metafields for Store, Customers, Subscriptions](https://developer.rechargepayments.com/#list-metafields)
  - [One-time Products](https://developer.rechargepayments.com/#list-onetimes-alpha)
  - [Orders](https://developer.rechargepayments.com/#list-orders)
  - [Products](https://developer.rechargepayments.com/#list-products-beta)
  - [Shop](https://developer.rechargepayments.com/#retrieve-shop)
  - [Subscriptions](https://developer.rechargepayments.com/#list-subscriptions)
- Outputs the schema for each resource
- Incrementally pulls data based on the input state

## Quick Start

1. Install

    Clone this repository, and then install using setup.py. We recommend using a virtualenv:

    ```bash
    > virtualenv -p python3 venv
    > source venv/bin/activate
    > python setup.py install
    OR
    > cd .../tap-recharge
    > pip install .
    ```
2. Dependent libraries
    The following dependent libraries were installed.
    ```bash
    > pip install singer-python
    > pip install singer-tools
    > pip install target-stitch
    > pip install target-json
    
    ```
    - [singer-tools](https://github.com/singer-io/singer-tools)
    - [target-stitch](https://github.com/singer-io/target-stitch)
3. Create your tap's `config.json` file which should look like the following:

    ```json
    {
        "access_token": "YOUR_ACCESS_TOKEN",
        "start_date": "2019-01-01T00:00:00Z",
        "user_agent": "tap-recharge <api_user_email@your_company.com>"
    }
    ```
    
    Optionally, also create a `state.json` file. `currently_syncing` is an optional attribute used for identifying the last object to be synced in case the job is interrupted mid-stream. The next run would begin where the last job left off.

    ```json
    {
        "currently_syncing": "users",
        "bookmarks": {
            "addresses": "2019-06-11T13:37:55Z",
            "charges": "2019-06-19T19:48:42Z",
            "collections": "2019-06-18T18:23:58Z",
            "customers": "2019-06-20T00:52:46Z",
            "discounts": "2019-06-19T19:48:44Z",
            "metafields_store": "2019-06-11T13:37:55Z",
            "metafields_customers": "2019-06-19T19:48:42Z",
            "metafields_subscriptions": "2019-06-18T18:23:58Z",
            "onetimes": "2019-06-20T00:52:46",
            "orders": "2019-06-19T19:48:44Z",
            "products": "2019-06-11T13:37:55Z",
            "shop": "2019-06-19T19:48:42Z",
            "subscriptions": "2019-06-18T18:23:58Z"
        }
    }
    ```

4. Run the Tap in Discovery Mode
    This creates a catalog.json for selecting objects/fields to integrate:
    ```bash
    tap-recharge --config config.json --discover > catalog.json
    ```
   See the Singer docs on discovery mode
   [here](https://github.com/singer-io/getting-started/blob/master/docs/DISCOVERY_MODE.md#discovery-mode).

5. Run the Tap in Sync Mode (with catalog) and [write out to state file](https://github.com/singer-io/getting-started/blob/master/docs/RUNNING_AND_DEVELOPING.md#running-a-singer-tap-with-a-singer-target)

    For Sync mode:
    ```bash
    > tap-recharge --config tap_config.json --catalog catalog.json > state.json
    > tail -1 state.json > state.json.tmp && mv state.json.tmp state.json
    ```
    To load to json files to verify outputs:
    ```bash
    > tap-recharge --config tap_config.json --catalog catalog.json | target-json > state.json
    > tail -1 state.json > state.json.tmp && mv state.json.tmp state.json
    ```
    To pseudo-load to [Stitch Import API](https://github.com/singer-io/target-stitch) with dry run:
    ```bash
    > tap-recharge --config tap_config.json --catalog catalog.json | target-stitch --config target_config.json --dry-run > state.json
    > tail -1 state.json > state.json.tmp && mv state.json.tmp state.json
    ```

6. Test the Tap
    
    While developing the ReCharge tap, the following utilities were run in accordance with Singer.io best practices:
    Pylint to improve [code quality](https://github.com/singer-io/getting-started/blob/master/docs/BEST_PRACTICES.md#code-quality):
    ```bash
    > pylint tap_recharge -d missing-docstring -d logging-format-interpolation -d too-many-locals -d too-many-arguments
    ```
    Pylint test resulted in the following score:
    ```bash
    Your code has been rated... FILL IN LATER.
    ```

    To [check the tap](https://github.com/singer-io/singer-tools#singer-check-tap) and verify working:
    ```bash
    > tap-recharge --config tap_config.json --catalog catalog.json | singer-check-tap > state.json
    > tail -1 state.json > state.json.tmp && mv state.json.tmp state.json
    ```
    Check tap resulted in the following:
    ```bash
    The output is valid. FILL IN LATER.

    ```
---

Copyright &copy; 2019 Stitch