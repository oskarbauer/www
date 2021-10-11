@startuml Sequence
autonumber
hide footbox

participant PKI as "Manufacturer's PKI"
participant D as "Device"
participant CGW as "CoAP Gateway"
participant IS as "Identity Store"
participant DPS as "Device Provisioning Service"
actor C as "Customer"

== Production ==
PKI --> D: Set Manufacturer certificate

== Enrollment and Provisioning ==
C -> DPS ++:  Configure custom CA in the group enrollment
return
note left of D: Endpoint of Device Provisiong Service\nis preconfigured or set by the device's\napplication layer after the startup.
[-> D: Set Device Provisioning Service address
D -> DPS ++ #aqua: Connect and authenticate mutually
note right #aqua: TLS established with\nManufacturer Certificate
DPS --> DPS: Validate the device identity against the enrollment\nlist entry using standard X.509 verification.
group Request Provisioning Profile
D -> DPS++: Identity Certificate Signing Request
DPS --> DPS: Sign CSR using custom or self-signed CA
DPS --> D: Unique Identity Certificate
D -> DPS: Request plgd cloud provisioning data
DPS --> IS++: Request Authorization Code
return Authorization Code
return plgd Cloud endpoint, onboarding code, ...
deactivate DPS
end
group Self Provisioning
    D --> D: Establish Device Owner
    D --> D: Provision security configuration
    D --> D: Provision cloud configuration
end

== Registration ==
D -> CGW++  #aquamarine: Connect and authenticate mutually
note right #aquamarine: TLS established with\nIdentity Certificate
D -> CGW++: Sign Up
group OAuth2.0 Authorization Code Grant Flow
    CGW -> O ++: Verify and exchange authorization code for JWT access token
    return Ok\n(JWT Access Token, Refresh Token, ...)
end
CGW -> IS ++: Register and assign device to user
return Registered
return Signed up\n(JWT Access Token, Refresh Token, ...)

== Authorization ==
D -> CGW ++: Sign In
CGW -> CGW: Validate JWT Access Token
CGW -> IS ++: Is device registered?
return Ok
CGW -> EB: Subscribe to device & owner events
CGW -> RA ++: Declare device as online
return
return Signed In

== Certificate Renewal ==
D -> DPS++  #aquamarine: Connect and authenticate mutually
note right #aquamarine: TLS established with\nIdentity Certificate
DPS --> DPS: Validate the device identity against the enrollment\nlist entry using standard X.509 verification.
D -> DPS++: Identity Certificate Signing Request
DPS --> DPS: Sign CSR using custom or self-signed CA
return Unique Identity Certificate
deactivate DPS
D --> D: Remove old certificate
deactivate CGW
alt Custom reconnect callback
    [-> D: Reconnect using new Identity Certificate
else Automated reconnection after renewal
end
D -> CGW++  #aquamarine: Connect and authenticate mutually
note right #aquamarine: TLS established with\nIdentity Certificate
D -> CGW++: Sign In

@enduml
