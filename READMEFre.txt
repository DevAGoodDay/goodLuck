Pr.jsx
import React, { useEffect, useState } from 'react'
import ProjectComponent from './ProjectComponent'

const Projects = () => {
  const [projects, setProjects] = useState([])
  useEffect(()=> {
    fetch('/data.json')
      .then(response=>response.json())
      .then(data => setProjects(data))
  }, [] )

  return (
    <div className='projects-container'>
        <h1>Projects</h1>
        <div className='projects'>
            {projects.map(item => <ProjectComponent {...item}/>)}
        </div>
    </div>
  )
}

export default Projects

PrC.jsx
import React from "react"; 

const ProjectComponent = (props) => {
    return (
        <div className="project"> 
        
            <img src={props.img} />
            <h2>{props.title}</h2>
            <p>{props.description}</p>
            <div className="stack">
                {props.stack.map(element => <div>{element}</div>)}
            </div>

        </div>

    )
}

export default ProjectComponent