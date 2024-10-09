# TLS Lab

## What I learned

In this TLS lab, I learned how to inspect and analyze SSL/TLS traffic using Wireshark, focusing on key aspects of the TLS handshake such as protocol versions, cipher suites, and certificate chains. I also gained experience examining website certificates and security configurations directly from browsers like Firefox, identifying details such as the TLS version, key exchange algorithms, encryption methods, and certificate attributes like Subject Alternative Names (SAN) and Basic Constraints. Additionally, I explored concepts like HMAC vs. digital signatures, Certificate Transparency, and the role of Certificate Revocation Lists (CRLs) in maintaining secure communications.

## SSL/TLS Traffic Inspection in Wireshark

### Analyzing TLS Traffic

1. **Open the File in Wireshark:**
   - Use Wireshark on your SEED VM or personal computer.
   - Open the `tls6.pcapng` file provided for this lab.

2. **Locate the TLS Session:**
   - Find frame #332, which is the start of a TLS session (indicated by the ClientHello message).

3. **Filter the TLS Stream:**
   - Right-click on frame #332 and select **Follow** -> **TLS Stream**.
   - Close the pop-up window that shows the TLS stream.
   - Add `&& ssl` to the end of the filter field and click **Apply** to view the TLS handshake steps.

4. **Inspect the TLS Handshake Messages:**
   - Expand the fields in the Transport Layer Security section to view detailed information about each message.

### Use Wireshark packet capture to answer:

**What is the TLS version?**
   - **Answer:** Look at the ClientHello message. The TLS version is specified in this message. Example: TLS 1.2 or TLS 1.3.

**Find the "Server Hello". What version did the website choose?**
   - **Answer:** Find the ServerHello message in the TLS stream. The version chosen by the server will be listed. Example: TLS 1.2 or TLS 1.3.

**What Ciphersuite was chosen?**
   - **Answer:** Look for the Ciphersuite field in the ServerHello message. It will list the encryption algorithms used. Example: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256.

**How many certificates were sent over during handshake?**
   - **Answer:** Count the number of certificate messages in the handshake process. Example: If you see three certificates, the answer is 3.

**What’s the Subject Common Name for the server certificate?**
   - **Answer:** Locate the server certificate and find the Subject Common Name (CN). Example: www.microsoft.com.

**What’s the Subject Common Name for the first intermediary certificate?**
   - **Answer:** Find the first intermediary certificate and note its Subject Common Name. Example: Microsoft RSA TLS CA 01.

**What’s the Subject Common Name for the Root CA certificate?**
   - **Answer:** Identify the Root CA certificate and its Subject Common Name. Example: DigiCert Global Root CA.

**Why is the “Finished” message missing?**
   - **Answer:** The "Finished" message might be missing due to:
     - The server/client has more messages to send.
     - The finished message is encrypted.
     - This is a TLS abbreviated handshake.
     - There is no finished message in the TLS handshake.
     - For most cases, it could be due to an abbreviated handshake or an encrypted message.

## SSL/TLS Inspection from Web Browser

### Steps for Inspection in Firefox:
1. Navigate to `https://www.citi.com/` in Firefox.
2. Click on the lock icon in the search bar, go to "Connection secure," and then "More information."

**TLS version**
   - **Answer:** Check the TLS version listed. Example: TLS 1.2 or TLS 1.3.

**Key Exchange and Authentication Protocol**
   - **Answer:** Look for protocols like ECDHE RSA, DH RSA, etc. Example: ECDHE RSA.

**Symmetric Protocol**
   - **Answer:** Identify the symmetric encryption protocol used. Example: AES 256 GCM.

**Hashing protocol**
   - **Answer:** Find the hashing algorithm used. Example: SHA256.

**The Root CA Common Name**
   - **Answer:** Identify the Root CA’s Common Name. Example: DigiCert Global Root G2.

**Subject Common Name (CN) of the website certificate**
   - **Answer:** Find the Subject Common Name of the website’s certificate. Example: www.citi.com.

**The first Subject Alternative Name (SAN)**
   - **Answer:** Locate the first SAN listed. Example: www.citi.com.

**Basic Constraints (from the certificate)**
   - **Answer:** Determine whether Basic Constraints are present and their values. Example: Yes or No.

## Inspection of etrade.com

### Steps for Inspection in Firefox:
1. Navigate to `https://us.etrade.com/home` using Firefox.
2. Follow the same steps as in Q2 to obtain the necessary information.

**TLS version**
   - **Answer:** Check the TLS version listed. Example: TLS 1.2.

**Key Exchange and Authentication Protocol**
   - **Answer:** Identify the protocol used. Example: ECDHE RSA.

**Symmetric Protocol**
   - **Answer:** Find the symmetric encryption protocol used. Example: AES 128 GCM.

**Hashing protocol**
   - **Answer:** Locate the hashing algorithm used. Example: SHA384.

## Inspection of coinbase.com

### Steps for Inspection in Firefox:
1. Navigate to `https://www.coinbase.com/` using Firefox.
2. Follow the same inspection steps as in Q2 and Q3.

**TLS Version**
   - **Answer:** Check the TLS version listed. Example: TLS 1.3.

**Symmetric Protocol**
   - **Answer:** Identify the symmetric encryption protocol used. Example: AES 256 GCM.

**Hashing Protocol**
   - **Answer:** Find the hashing algorithm used. Example: SHA256.

## TLS Questions

**Difference between HMAC and a Digital Signature**
   - **Answer:**
     - Digital Signature uses asymmetric keys whereas HMAC uses symmetric keys.
     - Digital Signatures can be used without first sharing a secret key.

**Certificate Transparency**
   - **Answer:**
     - It provides an open framework for monitoring and auditing SSL certificates.
     - It aims to remedy certificate-based threats by making the issuance and existence of SSL certificates open to scrutiny by domain owners, CAs, and domain users.
     - It aims to protect users from being fooled by certificates that were mistakenly or maliciously issued.

**Field Determining What Websites Certificate Can Be Used For**
   - **Answer:** Subject Alternative Name Extension

**Field Specifying CA or End Entity**
   - **Answer:** Basic Constraints: CA Extension

**Field Specifying CRL Location**
   - **Answer:** CRL Distribution Points Extension

**What is a Certificate Revocation List (CRL)?**
   - **Answer:** A list of certificates that have been revoked by the issuing CA before their actual expiration date.

**Issuer CN Same as Field in Intermediary CA’s Certificate**
   - **Answer:** Subject Alternative Name Extension

**Messages Hashed in the Finished Message**
   - **Answer:** All prior handshake messages

**True Statements about Subject Alternative Name (SAN)**
   - **Answer:** (Choose all that apply, based on specific statements provided in your course material.)

**True Statements about Self-Signed Certificates**
   - **Answer:** (Choose all that apply, based on specific statements provided in your course material.)

**True Statements about Certificate Pinning**
   - **Answer:** Certificate pinning:
     - (Choose correct statements about certificate pinning based on your additional research.)

**Reason for Implementing HTTP Strict Transport Security (HSTS)**
   - **Answer:** To prevent man-in-the-middle attacks and enforce secure connections.
