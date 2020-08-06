def gsuite_userinfo(email):
    import pickle
    import os.path
    from googleapiclient.discovery import build
    from google_auth_oauthlib.flow import InstalledAppFlow
    from google.auth.transport.requests import Request

    # If modifying these scopes, delete the file token.pickle.
    SCOPES = ['https://www.googleapis.com/auth/admin.directory.group.readonly',
              'https://www.googleapis.com/auth/admin.directory.user.readonly',
              ]
    creds = None
    if os.path.exists('token.pickle'):
        with open('token.pickle', 'rb') as token:
            creds = pickle.load(token)
    # If there are no (valid) credentials available, let the user log in.
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file('credentials.json', SCOPES)
            creds = flow.run_local_server(port=0)
            # Save the credentials for the next run
            with open('token.pickle', 'wb') as token:
                pickle.dump(creds, token)

    service = build('admin', 'directory_v1', credentials=creds)
    results = service.users().list(customer='my_customer', orderBy="givenName", query=f"email={email}").execute()
    users = results.get('users')
    for item in users:
        keys = item.keys()
        # print(keys) "Show you all the keys you can filter from"
        primaryemail = item.get('primaryEmail')
        name = item.get('name')['fullName']
        suspend = item.get('suspended')
        lastlogintime = item.get('lastLoginTime')
        phone = item.get('phones')[0]['value']
        mfa = item.get('isEnrolledIn2Sv')
        address = item.get('emails')[1]['address']
        changepassword = item.get('changePasswordAtNextLogin')
        photo = item.get('thumbnailPhotoUrl')

        print(f"Full Name:{name}\nEmail:{primaryemail}\nUserSuspend:{suspend}\nLastLogin: "
              f"{lastlogintime}\nPhoneNumber:{phone}\nMFA:{mfa}"
              f"\nAliases:{address}"
              f"\nChangePasswordAtNextLogin: {changepassword}\nPhotoUrl:{photo}")


gsuite_userinfo('Enter Email Address')
