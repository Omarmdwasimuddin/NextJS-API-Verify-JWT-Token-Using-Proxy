## NextJS API---> Verify JWT Token Inside Proxy

### heading...
![](https://imgur.com/Guf4Mgu.png)
![](https://imgur.com/Yyhb8Xd.png)

```bash
import { jwtVerify } from "jose";
import { NextRequest, NextResponse } from "next/server";



export async function proxy(request:NextRequest) {
    
    if (request.nextUrl.pathname.startsWith('/api/profile')) {
        
        // verify token
        try {
            
            const reqHeader = new Headers(request.headers);
            const Token = reqHeader.get('token');

            if (!Token) {
                return NextResponse.json(
                    {status: "Fail", message: "No token provided"},
                    {status: 401}
                )
            }

            const Key = new TextEncoder().encode(process.env.JWT_KEY);
            const decoded = await jwtVerify(Token, Key);

            return NextResponse.next();
        } catch (error) {
            console.error("User unauthenticated:", error)
            return NextResponse.json(
                {status: "Fail", message: "Invalid user!"},
                {status: 401}
            )
        }
    }

}
```
---
