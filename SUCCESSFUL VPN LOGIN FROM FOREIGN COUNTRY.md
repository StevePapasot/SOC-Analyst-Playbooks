The user has successfully authenticated to the company VPN from an unusual or foreign country.

- The credentials were **valid.**
- The authentication completed successfully.
- The IP was geolocated outside the company’s normal operating region.

### Open and interpret the offense

- Find the source IP (Inside or outside the corporate range)
- Find the Destination IP (Internal or external)
- Find Username/User account (Does the user belong to a corporate domain?)
- Offense start and last event
- Event count and magnitude
- Rules triggered.

### **User Behavior & History**

- Look where the user was logged from for a previous time range. Use as filters the username and the event name.
- Previous login failures. Where there multiple failed attempts before success?

*Filter the username and the Successful login*

### Validate the event chain

### Source IP Investigation

- **Investigate → WhoIs Lookup”** or **“External Lookup”**
- Use the “TryDetectThis” and find info about the IP
    - Check reputation manually ⇒ [abuseipdb.com](http://abuseipdb.com/), [virustotal.com](http://virustotal.com/), [ipinfo.io](http://ipinfo.io/)

### **Decision point**

At this stage, you’ll know:

- If the IP is **malicious or part of a VPN/proxy service → Suspicious.**
- If the IP is **clean and belongs to a legitimate ISP in a country where the user might be traveling → Possibly FP.**

<img width="907" height="626" alt="image" src="https://github.com/user-attachments/assets/a40ed55a-5311-4e0f-a283-d4972e3dfdf1" />
