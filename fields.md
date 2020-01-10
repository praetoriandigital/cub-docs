## Special fields

* <a name="organization"></a>**organization**: Organization selector with autosuggestions.
  If organization doesn't exist, additional fields for a new organizaiton will show. Initial value will be users current organization if available.

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

  To include Postal Code field in the additional fields for creation or updating
  of an organization, set the `requirePostalCode: true` option:

  ```js
      ...
      {
        type: 'organizaiton',
        requirePostalCode: true,
      },
      ...
  ```

* **organization_search**: Organization name input with autosuggestions.
  On submit, this field will contain text entered by user or name of found organization.
  Usage example: [cub.policeone.com/org-search-demo/](https://cub.policeone.com/org-search-demo/)
  ```js
      ...
      {
        name: 'organization_name',
        type: 'organization_search',
        onSearchResult: function(org, setValues) {
          if (typeof org === 'object') {
            // if `org` is `object` - existent organization was found
            // use `setValues` to fill in other fields
            //   setValues({fieldName: value})
            //      fieldName - name of field present in this form
            //      value - desired value
            setValues({
              organization_uid: org.id, // if you need org_uid in submit data
                                        // save it in hidden field
              country: org.country,
              phone: org.phone,
            });
          } else {
            // if `org` not `object` but `string` - organization was not found
            // `org` will be text entered by user
            setValues({
              organization_uid: '', // drop org_uid in hidden field
            })
          }
        },
        nothingFoundMsg: "Existent organization was not found, but that's ok. " +
                         "Continue filling in other fields."
      },
      ...
  ```

* <a name="position"></a>**position**: Position selector with autosuggestions.
  Initial value will be user's current position if available.

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

* **image**: Image Upload control for Generic Form.
  It's used for uploading the image to S3.
  After that this control receives the image URL and posts it with
  other Generic Form fields.

  ```js
      ...
      {
        type: 'image',
        value: 'https://your-image-url.com', // set init image url (e.g. if Org Logo already set)
      },
      ...
  ```

* <a name="checkboxgroup"></a>**checkboxgroup**: Grouped checkbox inputs.
  Has 'Check/Uncheck all' button for user convenience.
  Checkboxes can be arranged into columns.


  ```js
      ...
      {
        name: 'newsletters',
        type: 'checkboxgroup',
        columns: 2,  // can be 1, 2 or 3; if omitted - 1 column will be used
        options: [
          ['mlt_uc4ca4238a0b9238', 'PoliceOne Member Newsletter'],
          ['mlt_u8f14e45fceea167', 'P1 Breaking News Alerts'],
          ['mlt_u98f13708210194c', 'P1 Tactical List Newsletter'],
          ['mlt_RgtfY0bPZ56vakO3', 'P1 Tech Product Alerts'],
          ['mlt_uc74d97b01eae257', 'P1 Technology Newsletter'],
        ],
        value: [  // pre-checked values
          'mlt_uc4ca4238a0b9238',
          'mlt_u8f14e45fceea167',
          'mlt_u98f13708210194c'
        ]
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
* radiogroup
* inputgroup
* hidden
