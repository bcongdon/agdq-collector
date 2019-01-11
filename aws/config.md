# EC2
* Region: us-east-1 (for historical reasons)
* Storage: 16GB
* OS: ubuntu 16.04
* Size: t2.medium
* Be sure to set "Auto-assign Public IP" to True

# Networking
* Make sure that the `monitoring` lambda is on the VPC and is associated with VPC's subnets
* Create a NAT Gateway for the VPC. Make a route table that maps `0.0.0.0/0` to the created NAT, and associate that with all the VPC's subnets
* There needs to be 2 route tables:
  * 1 that maps `0.0.0.0/0` to the VPC's inet gateway (for internal connections)
  * 1 that maps `0.0.0.0/0` to the created NAT (for external connections)
* More info about Lambda/VPC: https://aws.amazon.com/premiumsupport/knowledge-center/internet-access-lambda-function/

# Checklist
- [ ] Take a snapshot of the previous event if not already done
- [ ] Update the Hugo config to have the countdown for "next" event. Deploy countdown and snapshot-ed previous event.
- [ ] Make sure that `monitoring` lambda isn't timing out
- [ ] Setup cloud watch dashboard to track new EC2 instance
- [ ] Make sure you can connect to postgres over SSH
- [ ] Trigger a test alarm with `zappa invoke monitoring monitoring.test_alarm` and make sure it sends a text/email
- [ ] Manually invoke health checks and make sure they return "no error".
  - [ ] `zappa invoke monitoring monitoring.health_check_api`
  - [ ] `zappa invoke monitoring monitoring.health_check_databases`