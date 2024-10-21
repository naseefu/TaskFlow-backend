package com.task.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.task.dto.AddTeam;
import com.task.dto.AddTeamRequest;
import com.task.dto.JoiningRequestDTO;
import com.task.dto.Response;
import com.task.service.impl.TeamServiceImpl;

@RestController
@RequestMapping("/teams")
public class TeamController {

	@Autowired
	private TeamServiceImpl teamServiceImpl;
	
	@GetMapping("/get-all-teams/{userId}")
	public ResponseEntity<Response> getAllTeams(@PathVariable Long userId){
		Response response = teamServiceImpl.getAllTeamByUserId(userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@PostMapping("/add-team/{userId}")
	public ResponseEntity<Response> addTeam(@RequestBody AddTeam addTeam,@PathVariable Long userId){
		
		Response response = teamServiceImpl.addTeam(addTeam, userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	@PostMapping("/join-team-request/{userId}")
	public ResponseEntity<Response> joinTeamRequest(@RequestBody JoiningRequestDTO joiningRequestDTO,@PathVariable Long userId){
		
		Response response = teamServiceImpl.joinTeamRequest(joiningRequestDTO,userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	@GetMapping("/get-team-requests/{userId}")
	public ResponseEntity<Response> getTeamRequest(@PathVariable Long userId){
		
		Response response = teamServiceImpl.getTeamRequests(userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	/////////////////////////////////////////////////////////////////////////////////////////////
	
	
	@GetMapping("/get-team-req/{userId}")
	public ResponseEntity<Response> getTeamRequestCount(@PathVariable Long userId){
		
		Response response = teamServiceImpl.getTeamRequests(userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	@PostMapping("/accept-team-request/{userId}/{teamId}")
	public ResponseEntity<Response> acceptTeamRequest(@PathVariable Long userId,@PathVariable Long teamId){
		
		Response response = teamServiceImpl.acceptTeamRequest(userId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	@PostMapping("/reject-team-request/{userId}/{teamId}")
	public ResponseEntity<Response> rejectTeamRequest(@PathVariable Long userId,@PathVariable Long teamId){
		
		Response response = teamServiceImpl.rejectTeamRequest(userId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	/////////////////////////////////////////////////////////////////////////////////////////////
	
	
	@GetMapping("/get-teamby-teamid/{userId}/{teamId}")
	public ResponseEntity<Response> getTeamByTeamId(@PathVariable Long userId,@PathVariable Long teamId){
		
		Response response = teamServiceImpl.getTeamByTeamId(userId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
		
	}
	
	//////////////////////////////////////////////////////////////////
	
	
	@PostMapping("/send-addmemberrequest")
	public ResponseEntity<Response> addMemberRequest(@RequestBody AddTeamRequest addTeamRequest){
		Response response = teamServiceImpl.addMemberRequest(addTeamRequest);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	//////////////////////////////////////////////////////////
	
	@GetMapping("/get-recruit-request/{userId}")
	public ResponseEntity<Response> getAllRecruitRequest(@PathVariable Long userId ){
		Response response = teamServiceImpl.getRecruitRequests(userId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@PostMapping("/accept-recruit-request/{userId}/{teamId}")
	public ResponseEntity<Response> acceptRecruitRequest(@PathVariable Long userId,@PathVariable Long teamId){
		Response response = teamServiceImpl.acceptRecruitRequest(userId, teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@PostMapping("/reject-recruit-request/{userId}/{teamId}")
	public ResponseEntity<Response> rejectRecruitRequest(@PathVariable Long userId,@PathVariable Long teamId){
		Response response = teamServiceImpl.rejectRecruitRequest(userId, teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	//////////////////////////////////////////////////////////////////////

	@PostMapping("/remove-member/{userId}/{removerId}/{teamId}")
	public ResponseEntity<Response> removeMemberFromTeam(@PathVariable Long userId,@PathVariable Long removerId,@PathVariable Long teamId){
		Response response = teamServiceImpl.removeMemberFromTeam(userId,removerId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
	
	@PostMapping("/delete-team/{userId}/{teamId}")
	public ResponseEntity<Response> deleteTeam(@PathVariable Long userId,@PathVariable Long teamId){
		Response response = teamServiceImpl.deleteTeam(userId,teamId);
		return ResponseEntity.status(response.getStatusCode()).body(response);
	}
}
