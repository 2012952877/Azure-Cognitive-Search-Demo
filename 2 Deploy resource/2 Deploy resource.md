### Deploy resource

#### a. Deploy search service

First, you need to register and log in to your azure portal.

![image-20200924103739014](https://i.loli.net/2020/09/24/9ns7S18kzpVKo53.png)

Click "Create a resource", then choose azure cognitive search and fill in some information, such as search service url and pricing tier.

![image-20200924103903695](https://i.loli.net/2020/09/24/ThAaJmtgi3IUMYD.png)

![image-20200924104151289](https://i.loli.net/2020/09/24/VtoAQEKBhIDj5pJ.png)

#### b. Deploy database

As an example, we use Cosmos database to store our sample data.

Similarly, find and create a Cosmos database in the portal.

![image-20200924104435486](https://i.loli.net/2020/09/24/RLJwSqg57TciG6H.png)

You also need to fill in some basic information, please make sure use Core(SQL) API and put your database in the same location as your search service.

![image-20200924104541280](https://i.loli.net/2020/09/24/7lP3i1cETFHJ52j.png)