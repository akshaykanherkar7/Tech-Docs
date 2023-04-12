
# Super Token Setup

Supertoken Website: https://supertokens.com

## Recepies used from supertoken
We use the multi factor authentication from Supertoken (https://supertokens.com/docs/mfa/introduction). In MFA the 2 factors are
  1. Passwordless ( https://supertokens.com/docs/passwordless/introduction ) - to support mobile number and OTP
     1. Results in a session created with second factor false
     2. Access to onboarding API's only with mobile OTP - Helps usage in web and mobile apps
  2. emailpassword ( https://supertokens.com/docs/emailpassword/introduction ) - emailpassword where password is the mpin. 
     1. All Loan related and non onboarding API's should be accessible
     2. Everytime a user closes kills app, user is loggedout of emailpassword session. 
   
## SuperToken Setup
  Zype implementation of SuperToken consists of 3 parts
     1. Supertoken Core application that handles authentication
     2. **supertoken** service that interfaces with SuperToken core
     3. Setting up of supertoken app in all the service for authentication. 
1. ## SuperToken Core setup
  Follow the following setup instructions for docker installation ( https://supertokens.com/docs/emailpassword/pre-built-ui/setup/database-setup/mysql ). We will integrate SuperToken to mySql common database. The database name is **supertoken**. 

  ### SuperToken Core setup
  The flags to be setup for docker. 
  ```code
    -p 3567:3567 \
    -e MYSQL_USER="username" \
    -e MYSQL_PASSWORD="password" \
    -e MYSQL_HOST="host" \
    -e MYSQL_PORT="3306" \
    -e MYSQL_DATABASE_NAME="supertokens" \
    -e API_KEYS=<comma seperated api keys> \
    -e PASSWORDLESS_CODE_LIFETIME=600000 \
    -e PASSWORD_HASHING_ALG=BCRYPT \
    -e LOG_LEVEL=DEBUG \
    -e BCRYPT_LOG_ROUNDS=11 \

    -d registry.supertokens.io/supertokens/supertokens-mysql
  ```
  1. PASSWORDLESSS_CODE_LIFETIME - https://supertokens.com/docs/passwordless/common-customizations/change-code-lifetime
  2. API_KEYS - https://supertokens.com/docs/passwordless/common-customizations/core/api-keys 
  3. BCRYPT_HASHING = https://supertokens.com/docs/emailpassword/common-customizations/password-hashing/bcrypt. Note we need to run the tuning of bcrypt tuning according to the hardware we have to set BCRYPT_LOG_ROUNDS
  4. LOG_LEVEL - https://supertokens.com/docs/emailpassword/common-customizations/core/logging. DEBUG for development and INFO for production
  Additional parameters to be added to the above config
  5. We will allow supertoken to create all the tables. 

  ### Nginix reverse proxy setup

  Follow instructions as in https://supertokens.com/docs/passwordless/common-customizations/core/add-ssl-via-nginx for Nginix reverse proxying port 3567 to https://authl8r.in . 

  ### CLI
    Supertoken default provides a CLI to manage supertoken actions on the server - https://supertokens.com/docs/passwordless/common-customizations/core/add-ssl-via-nginx 

  ### Supertoken Admin
  Supertoken user management dashboard is configured and exposed. https://supertokens.com/docs/userdashboard/about. It is accessible on https://auth.l8r.in/auth/dashboard. The access requires API key and hence will be available only to limited participants. 

  ### Supertoken Cors Management
  
  Even though we are reverse proxying supertoken behind Nginix, we need configure CORS - https://supertokens.com/docs/passwordless/troubleshooting/cors-issues
  1. For Mobile App - Cors does not matter
  2. For Web and partner portal https://zype-prod.co.in and https://zype-dev.co.in should be allowed. 
  3. We will not invoke superToken endpoint using any standard library other than supertoken library. 

  ### Super token API
Supertoken exposes wrappers for Axios to self handle session and other aspects. We will always use supertoken axios wrapper for communicating with the server. Supertoken API and data formats / schemas are provided - https://app.swaggerhub.com/apis/supertokens/CDI/2.16.0#/message


