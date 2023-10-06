# Process local files with Langchain

Sample Node.js app that uses OpenAI to process local data using Langchain
This is build gradually on https://github.com/ChunAllen/langchain-local
Medium Article: https://medium.com/@allenchun/feed-local-data-to-llm-using-langchain-node-js-fd7ac44093e9

### Added text descriping naval battle (laivanupotus) (Martti Meri)
- Original data  is still docs/data.txt
- Naval battle game is explained in data1.txt
- Extra running prompts in the end

### How it works
- Langchain processes it by loading documents inside docs/ (In this case, we have a sample data.txt)
- It works by taking big source of data, take for example a 50-page PDF and breaking it down into chunks
- These chunks are then embedded into a Vector Store which serves as a local database and can be used for data processing

### Prerequisites
- Node.js <= 18

### Setting Up
- npm install
- mv .env.copy .env
- Replace `OPEN-API-KEY` in .env with your actual API Key from OpenAPI


### Running prompts 
- Asking questions related to the document (origina = data.txt)
```
$ node index.js "Describe this applicant's employment history"
{
  text: ' This applicant has 5+ years of experience in IT, with experience in System Administration, Network Configuration, Software Installation, Troubleshooting, Windows Environment, Customer Service, and Technical Support. They worked as a Senior IT Specialist at XYZ Global from 2018-Present, an IT Support Specialist at Zero Web from 2015-2017, and a Junior Desktop Support Engineer at Calumcoro Medical from 2014-2015.'
}
```
- Asking questions not related to the document
```
$ node index.js "What is 1+1?"
{ text: " I don't know." }
```
...

- Asking questions having data.txt and data1.txt in the docs database
...
$ node index.js "How can a ship be destroyed?"
{
    text:   "A ship can be destroyed by being hit by a rocket, torpedo, or bomb launched from an attacking war machine. The attacking war machine can be a plane, submarine or ship, depending on the military setting, and the ammunition needs to reach the target and successfully detonate in order to declare victory in the battle and ultimately the war. In naval battles, the attacking war machine typically launches the rocket, torpedo, or bomb from the same altitude as the target ship's homebase, and the resulting explosion causes the ship to sink to the bottom of the sea. Other methods of destruction can include ramming from another vessel, weather or environmental damage, and sabotage from within the ship."
}
$ node index.js "What is my role?"
{
    text:   'Your role as an IT Specialist is to ensure the successful deployment of attacking war machines by monitoring the ammunition and providing technical assistance to ensure the ammunition reaches its target. You are also responsible for monitoring the progress of the battle to determine if you have won the battle or the war. With a BSc in Computer Science from the State University of New York and certifications in CompTIA A+ and MS Server, you have experience in system administration, network configuration, software installation, troubleshooting, working in a Windows environment, customer service, and technical support. Additionally, you have experience in maintaining, configuring, and monitoring Windows computers and peripherals, finding ways to cut equipment costs, installing desktop computers, improving overall network capabilities, spearheading hardware and software upgrade rollouts, and writing scripts to automate scheduled system patching.'
  }

### What if we want to reference Langchain using our local data and OpenAI LLM
```
const chain = new RetrievalQAChain({
  combineDocumentsChain: loadQARefineChain(model),
  retriever: vectorStore.asRetriever(),
});
```