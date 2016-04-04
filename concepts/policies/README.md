# Policies



## What is a Policy

### Introduction
Policy are <strong>rules</strong>, that  yo can apply on <strong>Controllers</strong> and <strong>Functions</strong>.
Policies provide <strong>access controll</strong>, <strong>autorization</strong> or any <strong>processing</strong> that can be done before accessing controller function.

You can use Policies for whatever you want, the only convention is that policies hat to been RULES.

In some case when you get complex treatment the best practice is to get short lines and undertandable code in policy, the checks and bool functions or processing into Services section of your app.

### Examples
For a policy that named isAdmin, we could write a function that allow access if user is admin and return a 403 if user is not an admin.

An other example can be a processing before access the controller, like refreshTokens, that automaticaly refresh authentication tokens for a user, if the user has not disconect from app, have same ip and has an outdated token that is valid, the result can be let accessing controllers or return a 403 if the user has disconected.    

## How to use it in Trails
