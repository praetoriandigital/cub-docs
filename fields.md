## Special fields

* **organization**: Organization selector with autosuggestions.
  If organization doesn't exist - will show additional fields for new organizaiton. Initial value will be users current organization if available.

  **IMPORTANT**: most likely you should use **position** field alongside **orgainzation** field.

  **IMPORTANT**: saved value will be organization ID of chosen or created organization
  ```js
      ...
      {
        type: 'organizaiton',
        // name: 'organization', // name is implicit, you CAN'T change it
        value: 'org_r0DY7pGnsSkUpZsM', // MUST BE valid organization uid
                                       // or value will be ignored and dropped
      },
      ...
  ```
* **position**: Position selector with autosuggestions.
  Initial value will be users current position if available.

  **IMPORTANT**: most likely you should use **position** field alongside **orgainzation** field. Based on combofox field.
  ```js
      ...
      {
        type: 'position',
        // name: 'position', // name is implicit, you CAN'T change it
        // value: 'most likely DONT DO THIS', // value most likely SHOULD NOT be set
        tags: ['Fire', ...] // array of tags to narrow down postitions list
                            // (will show positions related only to this tags)
                            // if not set - site tags will be used
                            // if organization chosen - its tags wil be used  
      },
      ...
  ```
* **country**: Country selector with autosuggestions.

  **IMPORTANT**: most likely you should use **state** field alongside **country** field. Based on combofox field.
  ```js
      ...
      {
        type: 'country',
        // name: 'country', // name is implicit, you CAN'T change it
        restrictToKnown: true, // restrict only to known by CUB countries
                               // which means - country must be chosen from dropdown list
      },
      ...
  ```
* **state**: State selector with autosuggestions based on chosen country. Based on combobox field.

  ```js
      ...
      {
        type: 'state',
        // name: 'state', // name is implicit, you CAN'T change it
        country: 'Uganda' // states of which country to suggest
                          // default value: 'United States'
        restrictToKnown: true, // restrict only to known by CUB states
                               // which means - state must be chosen from dropdown list
      },
      ...
  ```


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
