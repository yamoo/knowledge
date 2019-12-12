# AWS

## インターネットゲートウェイ(IGW)とNATゲートウェイの違い(NAT GW)
https://milestone-of-se.nesuke.com/sv-advanced/aws/internet-nat-gateway/

### IGW

- プライベート IP とパブリック IP が 1 対 1 で変換 (Static NAT)
- 「インターネットからのアクセス」と「インターネットへのアクセス」の両方を実現する

### NAT GW

- プライベート IP とパブリック IP が多対 1 で変換される
- プライベート IP が一意に定まらなくなってしまうので、TCP/UDP ポート番号も変換する
- 「インターネットからはアクセスされたくないけどインターネットへのアクセスは実施したい」というケースで有効
