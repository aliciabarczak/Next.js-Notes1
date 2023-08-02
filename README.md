# File Based Routing 

 - React uses additional libraries like router-dom and react-router- dom to allow 'routing' in SPAs. 
 - Next integrates these libraries within its framework so they do not need to be installed separately.
 - In Next you create React component files and let Next.js infer the routes from the folder structure

![screenshot-gluki udemy com-2023 07 31-11_57_09](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/393a9c3e-1173-4a05-b7e7-19d3d2077944)

## Gaining access to dynamic values in dynamic routing

### Client Side

 - in a route "domain.com/somehting/[id] - how do you get access to id?
 - you can use the Next useRouter hook which is imported from the next/router package - this is a React hook defined by the Next.js team that can be used in any functional component (you can us withRouter HOC if working with class components)
 - the useRouter hook provides various data and methods: 

 - *pathname* - provides you with the URL **inferred by Next.js** from the folder structure. For example "/something/[id].
 -  *query* - returns an object containing a key for any concrete data encoded in the URL. For example: 
	 - path like `something/[id]` encoded as `something/1` would return `{id: "1"}`
	 - path like `something/[id]/[clientProjectId]` with URL of  `something/1/5` would return `{id: "1", clientProjectId: 5}`.

![screenshot-gluki udemy com-2023 07 31-12_35_30](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/d3e40423-4ee0-4071-a8ad-b4f8744c43a3)

On the client side, it would also make sense to handle what happens if the data that is being fetched is either loading or unavalibale. Can use an if statement for that. 

### Catch all Routes

<img src="https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/537b7b45-27bf-429e-818e-a5ffa552b6f1" width="300"/>

 - using the spread operator and the value name of your choice, you can catch all URL fragments that in this case, would follow `domain.com/blog`.
 - For example, using `useRouter` and `query` property on `domain.com/blog/2022/09` would return an object like this: `{slug: ["2022", "12"]}`.
 - the catch all routes file will only be executed if the number of dynamic values in the URL is sufficient for its execution. So for example, if the [...slug].js file sits on the same level as [id].js file in 'something' folder, then any URL with domain.com/somehting/1246 will trigger the latter. Only URLs with more than one dynamic value following soehting will trigger the former. For example, domain.com/1233/12334.  

## Navigation

 - You need to use the Link component from next/link which takes href as an attribute. 
 - Link has advantage over the anchor tag in that it does not send out a new http request everytime the link is clicked and therefore the state of the application is preserved. 
 - to generate dynamic links, you can simply fetch all the data on which the links will be based and map over the same to create link for each peace of data.

![screenshot-gluki udemy com-2023 07 31-14_07_44](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/55722112-9994-43ed-ae6c-f542bcfa7878)


### Navigating programatically i.e. when on want to navigate to a page upon certain event happening such as a form being submitted for example

 - Link would not work here as clicking the link does not allow you to perform additional actions such as submitting a form. Link is used to simply navigate to a new page and does nothing else.
 - in order to navigate to a new page programatically upon certain event happening, you can use the useRouter hook's push method. You can declare the hook at the top level of the component and then use its return value inside of a handleClickFunction which is attached to a submit button: 

![screenshot-gluki udemy com-2023 07 31-14_21_00](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/2b040358-49e7-4089-b227-94997b2bf126)

## Custom 404 Page

Just add a 404.js / 404.tsx file to the root of the pages folder and default export the desired component. 

## General Layout

The_app file renders the entire application. This means that in this file, you can import the global css as well as any components that you want to appear in the entire application. 

For example, to add a header to the entire application:

 1. create a Header component:![screenshot-gluki udemy com-2023 07 31-20_34_57](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/e2fa3072-95d3-43ef-a286-189e794d8ea9)

    
 2. create a Layout component
![screenshot-gluki udemy com-2023 07 31-20_36_20](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/d38431f4-759f-43f9-a7be-e2c1779dfd2a)
    
 3. in _app, wrap the Component file in the Layout comp
![screenshot-gluki udemy com-2023 07 31-20_37_15](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/11dd0045-c29d-42dd-b41d-fdbea14bcd9f)

## Sort-by Pages

If you want to generate a page which is based on some sorting criteria, one way to do this in Next is to set the URL of the page based on the selected criteria (in a drop down menu for example) and navigating to that page upon submission. Once navigated to that page [...slug] page will be rendered and that page can be set to fetch and display data based on the URL fragments. 

 - create the sort by menu component which should trigger a hander on submission
   
 - the![screenshot-gluki udemy com-2023 08 01-13_01_08](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/a9183a98-4945-4138-bb7b-222a23367f65)
   
- handler should prevent default behaviour as well as keep track of the values that are being submitted. This can be done using the useRef hook if working in the client.
  
![screenshot-gluki udemy com-2023 08 01-13_01_46](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/9318eadd-e988-4c07-8349-f63e4fd662e8)

 - the hander should then invoke sorting function with the values. This function comes as a prop from the parent component which is a page where the sorting menu is rendered.
 - in the parent component the sorting function should use the values it is invoked with to navigate to a new page with the url matching the values passed.

![screenshot-gluki udemy com-2023 08 01-13_04_47](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/9ca1762a-bfba-43c1-8dd9-1258fc0813c0)


 - the [...slug] page should then grab the URL values - using useRounter query if on the client - to fetch data based on the invoked values and then render the same on the page. 
 - remember that the useRouter [...page] will always return an array with all the queries not an key/value pair like in all other pages.

>  note that in this method, this component will be rendered twice. The first time it will be rendered without the url and only upon the second time will the URL be available so need to take account of that when fetching any data.
>  Also always need to handle errors for this type of scenario. For example, if the fetching method expects certain values like numbers, you's only want to invoke the function if the correct values have been entered in the URL. An example of error handler could look like this:
> 
> ![screenshot-gluki udemy com-2023 08 01-13_14_58](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/0fc02c84-849a-46d6-9f11-adad10055ef4)
> 
> You also want to handle a situation of what happens when the fetch does not return any data:
> 
> ![screenshot-gluki udemy com-2023 08 01-13_17_22](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/0d32c109-9fff-4505-84ad-fbfafde67f3a)


### Static Generation vs Incremental Static Generation vs Server Side Rendering vs Server Side Components

 - SG happens when you pre-render the HTML/DOM and all of the data that is required on the load, on the server in advance/during **built time and before deployment** 
	 - So all pages which are built to be statically generated/pre-built in advance at load time and remain of the same content unless you include Incremental Static Generation or run the build again with updated content. 
	 - Because SG pages are pre-generated at build time, once deployed, they can be cached by server/CDN serving the app hence requests can be served instantly. 
	 - Once these pages are served, they are hydrated with JS/React to make them interactive. 
	 - You can tell Next that you want your pages to be pre-generated by exporting certain functions from the Pages directory in Next.js app - `export async function getStaticProps(context){ ...}` - in there you can run any code that you would normally run on the server side only, any dependencies not included in the client bundle, any credentials never make it to the client, you can use node objects and access the system to read files etc. 
 - ISG is like SG but it updates at set intervals
 - SSR is rendering pages on the server upon http request 
 - SSC are react components that only ever run on the server. They can include ready-made SSC like getStaticProps and custom components. By default, all components inside App directory are server components. Server components can have client components as children but clinet components cannot have server components as children.

# Static Generation in Next.js

 - will happen **automatically** if you create/export a component inside of Pages folder the without dynamic data: 
 - will also happen automatically if you export getStaticProps from a Pages file

## Get Static Props

 - executed once at built time
 - during the built process, it runs before the component it serves;
 - only ever runs on the server unlike the component it serves which will be part of the client code;
 - any imports used only within getStaticProps will be stripped out of the client bundle/allows for easy code splitting
 - allows you to access libraries/Node objects that you use on the server like fs or process;
 - when the getStaticProps is executed by Next, Next will always assume that the file sits in the root of the directory and not in the Pages folder;
 - once built, the HTML will contain a script tag that will contain all of the data passed between the getStaticProps and the served component; this is necessary to ensure that during the hydration process, React knows what data the pre-generated component was filled with so it can then work with that data.

   ![screenshot-gluki udemy com-2023 08 02-11_31_19](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/439b7805-a075-4586-8ab2-720e0a2403ed)

 ### notFound key
 - accepts a boolean 
 - if set to true, will render 404 page - why would you want that? It allows you to return an error page if for whatever reason the data you are fetching inside of the getStaticProps does not return the expected data.
   
   ![screenshot-gluki udemy com-2023 08 02-13_49_17](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/296aa1ef-295f-4e2b-a830-1e12474a1228)

## Build Process 

 - executing the build script will build the 'optimized production build'
 - once the command is run, Next will user side capabilities in the following ways: 

![screenshot-gluki udemy com-2023 08 02-11_36_19](https://github.com/aliciabarczak/NC_NEWS_NEXTJS/assets/101208108/22a11bd3-b37c-447f-a743-5b39dafe9833)

 - Static refers to any components that have hard coded return/do not use props/any data - pre-generated at built time - 404 page for example. 
 - SSG - static site generation is when you use getStaticProps and pass the same to the necessary component - pre-generated at built time
 - ISR - Incremental Static generation is when you use getStaticProps with the validate key - pre-generated at built time + re-validated at the set intervals 
 - SS - Server side is when the page is generated at run time at the time the http request is sent. 

 - You can further see the output of the build command in the .next folder. In the server subfolder you should have all of the html files which have been generated at build time.  
 - npm start - This will start the production ready page with a node.js server locally.

## Hosting implications
- none as does not run on the server which serves the application but rather a local node.js sever (created by Next) which builds the app on your machine when you execute npm run build. The static files are then hosted so no special node.js server hosting capability is required. 

 # Incremental Static Generation 

 - ensures that you do not just generate your page once statically at built time but it is continuously updated even after deployment without you re-deploying it. 
 - allows you to re-generate the page at every incoming request but at most by every 60 seconds (for example).
 - The page is re-created at most every 60 seconds.
 - If there are no requests for 3 days, the page will not be re-created every 60 seconds during those three days. The idea is that upon request, there is a capability to check how long ago the page was last generated and if it less than 60 seconds ago then the current page will be served otherwise a new page is generated on the server and sent to the clinet.
 - NOTE THAT IN THE DEVELOPMENT SERVER, YOU ALWAYS SEE THE LATEST VERSION OF THE PAGE AS THE PAGE IS ALWAYS RE-GENERATED WHEN YOU UPDATE YOUR FILES. IN PRODUCTION HOWEVER, IT WILL ONLYBE RE-GENERATED BASED ON WHAT INYERVALS YOU HAVE SET - However you can test production locally by running npm start which will start the local server sercing the app. This should behave like real production site and should only generate as per the intervals set which you can test with for exmaple, console.logs.

> 1. request has been made for a page. The page has last been generated 30 seconds ago. Current version is sent by the server.
> 2.  request has been made. The page has last been generated 65 seconds ago. A new page is generated by the server and cached and sent to the client replacing the old version.
> ![screenshot-gluki udemy com-2023 08 02-13_23_21](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/0261350f-ba47-4aa0-86c9-cfb528a7a9a6)

 - Alternative to using this method would be to use Static Generation for the initial load and then on the client, use useEffect to fetch any updates to the page from the server;

## Syntax 

![screenshot-gluki udemy com-2023 08 02-13_37_47](https://github.com/aliciabarczak/Next.js-Notes1/assets/101208108/c28d11a0-a363-4bc7-84f0-cba5e408256f)

YOU CAN ALWAYS CHECK WHETHER NEXT HAS PICKED UP THE KEY BY RUNNING THE BUILD AND MAKING SURE THAT THE OUTPUT IN THE TERMINAL REGISTERED THAT ISG HAS BEEN INTRODUCED FOR THAT PAGE AND THEREFORE THE PAGE WILL BE RE-GENERATED AS PER THE INTERVAL SPECIFIED.

