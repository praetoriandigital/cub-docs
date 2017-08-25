## Special fields

* **organization**: Organization selector with autosuggestions.
  If organization doesn't exist - will show additional fields for new organizaiton. Initial value will be users current organization if available.

  **IMPORTANT**: most likely you should use **position** field alongside **orgainzation** field.

  **IMPORTANT**: saved value will be organization ID of chosen or created organization
  ```js
      ...
      {
        type: 'organizaiton',
        name: 'organization', // name MUST be 'organization'
        //value: 'DONT DO THIS', // value MUST NOT be set
      },
      ...
  ```
* **position**: Position selector with autosuggestions.
  Initial value will be users current position if available.

  **IMPORTANT**: most likely you should use **position** field alongside **orgainzation** field.
  ```js
      ...
      {
        type: 'position',
        name: 'position', // name MUST be 'position'
        //value: 'most likely DONT DO THIS', // value most likely SHOULD NOT be set
        tags: ['Fire', ...] // array of tags to narrow down postitions list
                            // (will show positions related only to this tags)
                            // if not set - site tags will be used
                            // if organization chosen - its tags wil be used  
      },
      ...
  ```
* **country**: Country selector with autosuggestions.

  **IMPORTANT**: most likely you should use **state** field alongside **country** field.
  ```js
      ...
      {
        type: 'country',
        name: 'country', // name MUST be 'country'
        restrictToKnown: true, // restrict only to known by CUB countries
                               // which means - country must be chosen from dropdown list
      },
      ...
  ```
* **state**: State selector with autosuggestions based on chosen country.

  ```js
      ...
      {
        type: 'state',
        name: 'state', // name MUST be 'state'
        country: 'Uganda' // states of which country to suggest
                          // default value: 'United States'
        restrictToKnown: true, // restrict only to known by CUB states
                               // which means - state must be chosen from dropdown list
      },
      ...
  ```
* **register_me**: TODO


## Other fields

* text
* textarea
* select
* combobox
* checkbox
* checkboxgroup
* radiogroup
* inputgroup
* hidden
