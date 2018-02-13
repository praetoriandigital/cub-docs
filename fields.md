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
* **multiselect**: Multiselect control with autosuggestions based on react-widgets.

  **IMPORTANT**: The user should have the admin rights and be an active member of
  the organization he wanted to change.

  ```js
      ...
      const _options = [
        {"id":73770004, "title":"3D Laser Scanners"},
        {"id":94732, "title":"Accident Reconstruction"},
      ];
      const _value = _options[1];
      ...
      {
        type: 'multiselect',
        options: _options, // the data for Multi Select Control
        value: '', // init value
        valueField: 'id', // you could change the name value field. It depends on your data.
        textField: 'title', // you could change the name text field. It depends on your data.
      },
      ...
  ```
* **image**: Image Upload control for Generic Form. It's used for
  uploading organization logo.

  **IMPORTANT**: The user should have the admin rights and be active member of
  organization he wanted to change.

  ```js
      ...
      {
        type: 'image',
        value: 'org_r0DY7pGnsSkUpZsM', // MUST BE valid organization uid
      },
      ...
  ```


## Other fields

* text
* textarea
* select
* combobox
* multiselect
* checkbox
* image
* checkboxgroup
* radiogroup
* inputgroup
* hidden
