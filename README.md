# rupoc
POC for the assignment from the Swedish Government to Arbetsförmedlingen Jobtech to show case the individuals possibility to take control of her/his own data.

# Case Study
It was decided that the POC should show case a job application process where the individual could import data from some governmental agencies to enrich her/his CV-profile, and to allow an extern (private) job site to fetch that data and show it to an employer representative.

In the POC four data points were selected, residing at four different government agencies. Official address information from Skatteverket, study certificates from Ladok, an excerpt from the individuals criminal record from Polisen (to be used for job applications where this is mandatory - kondergarden etc), and driver license information from Transportstyrelsen. (The criminal record excerpt was later removed from the POC due to secrecy and current legislation reasons).

Two different architectures were found that we wanted to evaluate, a MyData based system where the individual could consent the usage of her/his data through a MyData Operator and its Wallet implementation, and a system based on a SOLID implementation where the individuals data were imported in a (hosted) personal data store PDS and could be shared at the users will.

# MyData implementation
For the MyData variant of the POC a solution from Vastuu group in Finland were chosen. Its (cloud hosted) MyData Share product offered an integrated system with a single sign on (SSO) implementation from Signicat and the possibility to log in with Swedish test BankId clients. The product also contained a Wallet where the individual could control the sharing of her/his data through consents, and the MyData Share Access Gateway AGW that were to be deployed in front of the data sources - controlling the access to the data.

The following diagram gives an oversite of the architecture:

![](MyData.svg)

Since there were no appropriate test services that could be used we created four mock services for the POC that could serve test data. One of the services, the navet mock service, are backed by a real test service at Skatteverket. (But it also had to be fronted by a mocked service since the MyData Share AGW only could front REST services and the Skatteverke test service is implemented with SOAP).

The Arbetsförmedlingen MinProfil application were mocked so it could show the creation of enriched CV profiles where data from the four mocked data sources could be fetched. In this case the back end mina-profiler mock service acted as a data using service from the MyData Shares point of view. The application were divided into a database backed backend and a frontend single page application for user interaction. Integration with MyData Share for login were done in the frontend code, and the backend had to be registered as a data using service and integratied with MyData Share to obtain rights to fetch and process information through the access gateways from the mock services.

The mina-profiler backend were also registered as a data source offering CV profiles as data, and it's export service is fronted by an access gateway.

Also an example Jobsite were mocked, implemented as a database backed backend and a frontend single page application. The Jobsite is registered as a data using service so the user can chose one of her/his CV profiles and make it accessible for the jobsite when applying for a job. The access of the chosen CV profile is being consented through the MyData Share Wallet, and can be tied to a specific job application and thereby available to be shown to enployer representatives.

| Repository                                                                                  |
| ------------------------------------------------------------------------------------------- |
| [rupoc](https://github.com/individdata/rupoc)                                               |
| [rupoc-mock-mina-profiler-spa](https://github.com/individdata/rupoc-mock-mina-profiler-spa) |
| [rupoc-mock-mina-profiler](https://github.com/individdata/rupoc-mock-mina-profiler)         |
| [rupoc-mock-navet](https://github.com/individdata/rupoc-mock-navet)                         |
| [rupoc-navet-client](https://github.com/individdata/rupoc-navet-client)                     |
| [rupoc-bankid-client](https://github.com/individdata/rupoc-bankid-client)                   |
| [rupoc-ladok-client](https://github.com/individdata/rupoc-ladok-client)                     |
| [rupoc-mock-ladok](https://github.com/individdata/rupoc-mock-ladok)                         |
| [rupoc-mock-trafikverket](https://github.com/individdata/rupoc-mock-trafikverket)           |
| [rupoc-mock-polisen](https://github.com/individdata/rupoc-mock-polisen)                     |
| [rupoc-mock-jobsite](https://github.com/individdata/rupoc-mock-jobsite)                     |
| [rupoc-infra](https://github.com/individdata/rupoc-infra)                                   |
