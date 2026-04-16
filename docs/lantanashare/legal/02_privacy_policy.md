# Privacy Policy

**LantanaShare** — Privacy Policy
**Effective Date:** April 15, 2026
**Last Updated:** April 15, 2026

---

## 1. Introduction

Welcome to **LantanaShare** ("we," "us," or "our"). We are committed to protecting your personal information and your right to privacy. This Privacy Policy explains how we collect, use, share, and protect your information when you use our mobile application and related services (collectively, the "Services").

Please read this policy carefully. By creating an account or using the Services, you agree to the practices described here. If you do not agree, please do not use our Services.

---

## 2. Information We Collect

### 2.1 Information You Provide to Us

- **Account Information:** When you register, we collect your name, email address, phone number, profile photo, and password.
- **Profile Information:** Location (city, neighborhood), community membership, and any additional details you choose to add to your profile.
- **Item Listings:** Information about Items you list for borrowing, including descriptions, photos, availability, rental pricing, and declared replacement cost.
- **Transaction Information:** Records of Item lending and borrowing activity, rental agreements, bill splitting calculations, and payment acknowledgments. Note that LantanaShare does not process payments directly — all payments are settled between users outside the app using their mutually agreed-upon payment method.
- **Communications:** Messages exchanged with other users within the app, dispute submissions, and support requests sent to us.

### 2.2 Information We Collect Automatically

When you use the Services, we automatically collect:

- **Usage Data:** Features you use, pages you visit, searches you perform, and actions you take within the app.
- **Device Information:** Device type, operating system, unique device identifiers, browser type, and mobile network information.
- **Location Information:** Approximate location (based on IP address or, if you grant permission, GPS data) to help connect you with your local community.
- **Log Data:** IP address, access times, app crashes, and referring URLs.
- **Analytics:** We use Firebase Analytics to understand app usage and diagnose errors. Data collected is aggregated and does not directly identify you.

### 2.3 Authentication Data

LantanaShare uses **email and password authentication** to verify your identity. When you log in, your email address and password are transmitted over a TLS-encrypted connection. Passwords are never stored in plaintext — only as a cryptographic hash (bcrypt) on our servers. Upon successful authentication, the app receives a short-lived access token (JWT) and a refresh token, both stored securely on your device. Access tokens expire automatically; refresh tokens are used to obtain new access tokens without requiring you to log in again.

We retain authentication tokens only for as long as your session is active. No authentication credentials are retained after account deletion.

### 2.4 Information We Receive from Third Parties

- **Social Login:** If you register or log in using a third-party account (e.g., Google or Apple), we receive basic profile information such as your name and email address. That information is governed by the third party's privacy policy.

---

## 3. How We Use Your Information

We use the information we collect to:

- **Provide and operate the Services**, including facilitating Item listings, connecting borrowers and lenders, and coordinating transactions.
- **Personalize your experience**, including showing you relevant Items, communities, and Members near you.
- **Authenticate your identity** securely using email and password authentication with encrypted session tokens.
- **Communicate with you**, including sending service notifications, payment notices, updates, and security alerts.
- **Manage disputes and bill splitting**, including sharing information with community admins to resolve conflicts between users, calculating monthly payment obligations between members, and recording payment acknowledgments between debtors and creditors.
- **Improve the Services** through analytics, research, and development of new features.
- **Ensure safety and security**, including detecting fraud, abuse, and violations of our Terms of Service.
- **Comply with legal obligations** and respond to lawful requests from authorities.

---

## 4. How We Share Your Information

We do **not** sell your personal information for monetary compensation. We do **not** share your personal information with advertisers or advertising networks.

We may share your information in the following circumstances:

- **With Other Users:** Your public profile (name, photo, community, and Item listings) is visible to other members of your community. Transaction details are shared between the relevant borrower and lender.
- **With Community Admins:** Admins of your community may access information relevant to dispute resolution, as described in the Community Bylaws.
- **With Service Providers:** We share information with third-party vendors who help us operate the Services. Current service providers include:
  - **Cloud hosting (AWS):** App data and services are hosted on Amazon Web Services.
  - **Image storage (AWS S3):** User-uploaded images (Item photos, profile photos) are stored in Amazon S3.
  - **Email delivery (Google Workspace / Gmail):** Used to send transactional emails such as payment notices and account alerts. Your email address is processed by Google's servers for this purpose.
  - **Push notifications (Google Firebase Cloud Messaging):** Used to deliver push notifications to your device. A device-specific FCM token is shared with Google to enable delivery.
  - **Analytics (Firebase Analytics):** Used to understand app usage and diagnose errors. Data is aggregated and does not directly identify you.
- **For Legal Reasons:** We may disclose your information if required by law, court order, or governmental authority, or to protect the rights, safety, or property of LantanaShare, our users, or the public.
- **Business Transfers:** If LantanaShare is involved in a merger, acquisition, or sale of assets, your information may be transferred as part of that transaction. We will notify you via email and/or in-app notification prior to such a transfer.
- **With Your Consent:** We may share your information for any other purpose with your explicit consent.

---

## 5. Data Retention

We retain your personal information for as long as your account is active or as needed to provide the Services. If you delete your account, we will delete or anonymize your personal information within **30 days**, except where we are required by law to retain it, or where retention is necessary to resolve disputes or enforce our agreements.

The following specific retention periods apply to different categories of data:

- **Account and profile information** is retained for the life of your account and deleted within 30 days of account deletion.
- **Item listings and rental records** are retained for the life of your account. After account deletion, anonymized aggregate records may be retained for platform integrity purposes.
- **Bill splitting and payment acknowledgment records** associated with resolved transactions are retained for **12 months** from the date of resolution for accounting and dispute reference purposes.
- **Dispute records and associated communications** are retained until the matter is fully resolved, and for **12 months** thereafter.
- **Authentication credentials:** Your hashed password is retained for the life of your account and deleted within 30 days of account deletion. Active session tokens (JWT) expire automatically. Refresh tokens are invalidated upon logout or account deletion.

---

## 6. Your Privacy Rights and Choices

Depending on where you live, you may have certain rights regarding your personal information, including:

- **Access:** You can request a copy of the personal information we hold about you.
- **Correction:** You can update or correct inaccurate information through your account settings.
- **Deletion:** You can request deletion of your account and personal information.
- **Withdraw Consent:** Where we rely on your consent to process data, you may withdraw it at any time.

**California residents** (and residents of Colorado, Connecticut, Virginia, Texas, Oregon, and other states with applicable privacy laws) may have additional rights. As LantanaShare does not sell or share personal information for advertising purposes, cross-context behavioral advertising opt-out rights are not currently applicable.

**EEA/UK residents** have rights under the General Data Protection Regulation (GDPR) or UK GDPR, including the right to object to processing based on legitimate interests.

To exercise any of these rights, please contact us at **ubertool320@gmail.com**.

---

## 7. Children's Privacy

Our Services are not directed to children under the age of 13 (or the applicable minimum age in your jurisdiction). We do not knowingly collect personal information from children. If we become aware that a child under 13 has provided us with personal information, we will take steps to delete it promptly.

---

## 8. Security

We implement commercially reasonable technical and organizational measures to protect your information against unauthorized access, loss, or misuse. Our security measures include:

- **Secure authentication:** LantanaShare uses email and password authentication. Passwords are transmitted only over TLS-encrypted connections and are stored only as cryptographic hashes (bcrypt) on our servers — your plaintext password is never stored. Upon login, the app receives a short-lived access token and a refresh token that authorize subsequent API requests.
- **Encrypted data transmission:** All communications between the app and our servers use TLS encryption (gRPC over TLS on port 50052).
- **Cloud infrastructure security:** Our cloud hosting provider (AWS) maintains physical and network security controls for the infrastructure on which your data is stored.

However, no security system is completely impenetrable, and we cannot guarantee the absolute security of your information. You are responsible for maintaining the security of your account password.

---

## 9. International Data Transfers

Our Services are operated from the United States. If you access the Services from outside the United States, your information will be transferred to and processed in the United States. Where required by law, we use appropriate data transfer mechanisms (such as Standard Contractual Clauses) to ensure adequate protection of your information.

---

## 10. Changes to This Policy

We may update this Privacy Policy from time to time. If we make material changes, we will notify you via email and/or a prominent in-app notification at least **30 days** before the changes take effect. Your continued use of the Services after the effective date of any update constitutes your acceptance of the revised policy.

---

## 11. Contact Us

If you have questions, concerns, or requests about this Privacy Policy or your personal information, please contact us:

**LantanaShare Privacy Team**
Lantana Intelligence LLC
Email: ubertool320@gmail.com
Support: ubertool320@gmail.com

---

*This Privacy Policy was last updated on April 15, 2026.*