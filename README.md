﻿<div align="center">

## Fast 64bit RSA Encryption Algorithm


</div>

### Description

The famous rsa public key encryption algorithm, this code is based on the original design by: Asgeir Bjarni Ingvarsson. Now includes source code and zip file with working example.
 
### More Info
 
rtn = enc(text, key(1), key(3)) - Encrypt

rtn = dec(enc_text, key(2), key(3)) - Decrypt

key(1) = e '(E)ncryptor (Public)

key(2) = d '(D)ecryptor (Private)

key(3) = n 'Modulus (Public and Private)

Returns encrypted or decrypted data in rtn

Modulus will handle a maximum of only 64bits


<span>             |<span>
---                |---
**Submitted On**   |2000-03-25 18:00:04
**By**             |[William Gerard Griffiths \(Author\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/william-gerard-griffiths-author.md)
**Level**          |Advanced
**User Rating**    |4.5 (36 globes from 8 users)
**Compatibility**  |VB 4\.0 \(32\-bit\), VB 5\.0, VB 6\.0
**Category**       |[Encryption](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/encryption__1-48.md)
**World**          |[Visual Basic](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/visual-basic.md)
**Archive File**   |[CODE\_UPLOAD4605472000\.zip](https://github.com/Planet-Source-Code/william-gerard-griffiths-author-fast-64bit-rsa-encryption-algorithm__1-6749/archive/master.zip)

### API Declarations

```
Public key(1 To 3) As Double
Public p As Double, q As Double
Public PHI As Double
Public Sub keyGen()
'Generates the keys for E, D and N
Dim E#, D#, N#
Const PQ_UP As Integer = 9999 'set upper limit of random number
Const PQ_LW As Integer = 3170 'set lower limit of random number
Const KEY_LOWER_LIMIT As Long = 10000000 'set for 64bit minimum
p = 0: q = 0
Randomize
Do Until D > KEY_LOWER_LIMIT 'makes sure keys are 64bit minimum
Do Until IsPrime(p) And IsPrime(q) ' make sure q and q are primes
p = Int((PQ_UP - PQ_LW + 1) * Rnd + PQ_LW)
q = Int((PQ_UP - PQ_LW + 1) * Rnd + PQ_LW)
Loop
 N = p * q
 PHI = (p - 1) * (q - 1)
 E = GCD(PHI)
 D = Euler(E, PHI)
Loop
 key(1) = E
 key(2) = D
 key(3) = N
End Sub
Private Function Euler(E3 As Double, PHI3 As Double) As Double
'genetates D from (E and PHI) using the Euler algorithm
On Error Resume Next
Dim u1#, u2#, u3#, v1#, v2#, v3#, q#
Dim t1#, t2#, t3#, z#, uu#, vv#, inverse#
u1 = 1
u2 = 0
u3 = PHI3
v1 = 0
v2 = 1
v3 = E3
Do Until (v3 = 0)
 q = Int(u3 / v3)
 t1 = u1 - q * v1
 t2 = u2 - q * v2
 t3 = u3 - q * v3
 u1 = v1
 u2 = v2
 u3 = v3
 v1 = t1
 v2 = t2
 v3 = t3
 z = 1
Loop
uu = u1
vv = u2
If (vv < 0) Then
  inverse = vv + PHI3
Else
 inverse = vv
End If
Euler = inverse
End Function
Private Function GCD(nPHI As Double) As Double
'generates a random number relatively prime to PHI
On Error Resume Next
Dim nE#, y#
Const N_UP = 99999999 'set upper limit of random number for E
Const N_LW = 10000000 'set lower limit of random number for E
Randomize
nE = Int((N_UP - N_LW + 1) * Rnd + N_LW)
top:
 x = nPHI Mod nE
 y = x Mod nE
 If y <> 0 And IsPrime(nE) Then
 GCD = nE
 Exit Function
 Else
 nE = nE + 1
 End If
 GoTo top
End Function
Private Function IsPrime(lngNumber As Double) As Boolean
'Returns 'True' if lngNumber is a prime
On Error Resume Next
Dim lngCount#
Dim lngSqr#
Dim x#
lngSqr = Int(Sqr(lngNumber)) ' Get the int square root
 If lngNumber < 2 Then
 IsPrime = False
 Exit Function
 End If
 lngCount = 2
 IsPrime = True
 If lngNumber Mod lngCount = 0 Then
 IsPrime = False
 Exit Function
 End If
 lngCount = 3
 For x = lngCount To lngSqr Step 2
 If lngNumber Mod x = 0 Then
  IsPrime = False
  Exit Function
 End If
 Next
End Function
Public Function Mult(ByVal x As Double, ByVal p As Double, ByVal m As Double) As Double
'encrypts, decrypts values passed to the function.. e.g.
'Mult = M^E mod N (encrypt) where M = x , E = p, N = m
'Mult = M^D mod N (decrypt)
On Error GoTo error1
y = 1
 Do While p > 0
 Do While (p / 2) = Int((p / 2))
  x = nMod((x * x), m)
  p = p / 2
 Loop
 y = nMod((x * y), m)
 p = p - 1
 Loop
 Mult = y
 Exit Function
error1:
y = 0
End Function
Private Function nMod(x As Double, y As Double) As Double
'this function replaces the Mod command. instead of z = x Mod y
'it is now z = nMod(x,y)
On Error Resume Next
Dim z#
z = x - (Int(x / y) * y)
nMod = z
End Function
Public Function enc(tIp As String, eE As Double, eN As Double) As String
'returns the long value of the characters, chained with a +
'e.g. 12345678+23456789+ etc..
'**Taken out encryption algorithm to simplify program**
On Error Resume Next
Dim encSt As String
encSt = ""
e2st = ""
 If tIp = "" Then Exit Function
 For i = 1 To Len(tIp)
 encSt = encSt & Mult(CLng(Asc(Mid(tIp, i, 1))), eE, eN) & "+"
 Next i
'** put your encryption algorithm code here **
enc = encSt
End Function
Public Function dec(tIp As String, dD As Double, dN As Double) As String
'returns the characters from the long values
'e.g A = 12345678, B = 23456789 etc..
'**Taken out decryption algorithm to simplify program**
On Error Resume Next
Dim decSt As String
decSt = ""
'** put your decryption algorithm code here **
For z = 1 To Len(tIp)
 ptr = InStr(z, tIp, "+")
 tok = Val(Mid(tIp, z, ptr))
 decSt = decSt + Chr(Mult(tok, dD, dN))
 z = ptr
Next z
dec = decSt
End Function
```





