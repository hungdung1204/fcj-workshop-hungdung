---
title: "Customer registration, login, profile update, and payment method"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

This test validates the customer account flow on the HaShop frontend. The purpose is to confirm that a new customer can create an account, verify the account by email, sign in, update profile information, and add a payment method before continuing to the order flow.

## Register a customer account

Open the HaShop registration page and enter the required customer information, including full name, email address, phone number, password, and password confirmation.

![Customer registration page](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image1.png)

After submitting the form, check the registered email inbox for the verification code. If the email does not appear in the inbox, check the spam or junk mail folder.

![Verification email](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image2.png)

Enter the OTP code on the email verification page to activate the customer account.

![Email verification page](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image3.png)

![Email verification completed](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image4.png)

## Log in with the customer account

After the account is verified successfully, return to the login page and sign in with the newly created customer account.

![Customer login page](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image5.png)

## Update profile and payment method

From the customer account area, open `Profile`, then choose the edit profile option.

![Customer profile page](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image6.png)

Update the customer information, add the delivery address, and link MoMo as the payment method.

![Edit profile and add payment method](/fcj-workshop-hungdung/images/5-Workshop/hashop-test-cleanup/customer-account/image7.png)

Expected result:

- The customer account can be registered successfully.
- The OTP verification email is received and can be used to verify the account.
- The customer can log in after successful verification.
- Profile information, address, and MoMo payment method can be saved successfully.
