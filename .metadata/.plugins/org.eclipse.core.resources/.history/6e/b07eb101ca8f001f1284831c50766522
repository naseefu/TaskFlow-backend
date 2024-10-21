package com.task.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.task.dto.AddProjectDTO;
import com.task.dto.ProjectDTO;
import com.task.dto.Response;
import com.task.entity.Project;
import com.task.service.impl.ProjectServiceImpl;

@RestController
@RequestMapping("/projects")
public class ProjectController {
	
	@Autowired
	private ProjectServiceImpl projectServiceImpl;
	
	@GetMapping("/get-all-projects/{userId}")
	public ResponseEntity<Response> getAllProjectByUserid(@PathVariable Long userId){
		Response response = projectServiceImpl.getAllProjectByUserId(userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@GetMapping("/get-projects-teamid/{userId}/{teamId}")
	public ResponseEntity<Response> getAllProjectByTeamid(@PathVariable Long userId,@PathVariable Long teamId){
		Response response = projectServiceImpl.getAllProjectByteamId(userId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@PostMapping("/add-project/{userId}/{teamId}")
	public ResponseEntity<Response> addProject(@PathVariable Long userId,@PathVariable Long teamId,@RequestBody AddProjectDTO addProjectDTO){
		
		Response response = projectServiceImpl.addProject(userId,teamId,addProjectDTO);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	@GetMapping("/get-each-project/{userId}/{teamId}/{projectId}")
	public ResponseEntity<Response> getEachProjectByProjId(@PathVariable Long userId,@PathVariable Long teamId,@PathVariable Long projectId){
		
		Response response = projectServiceImpl.getEachProjectByProjId(userId,teamId,projectId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}

}
