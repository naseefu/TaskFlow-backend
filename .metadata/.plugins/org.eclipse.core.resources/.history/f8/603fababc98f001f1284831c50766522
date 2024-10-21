package com.task.service.impl;

import java.security.SecureRandom;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.task.dto.AddProjectDTO;
import com.task.dto.ProjectDTO;
import com.task.dto.Response;
import com.task.dto.TeamDTO;
import com.task.dto.UserDTO;
import com.task.entity.Project;
import com.task.entity.Task;
import com.task.entity.Team;
import com.task.entity.TeamMembers;
import com.task.entity.User;
import com.task.exception.OurException;
import com.task.repository.ProjectRepository;
import com.task.repository.TaskRepository;
import com.task.repository.TeamMemberRepository;
import com.task.repository.TeamRepository;
import com.task.repository.UserRepository;
import com.task.utils.Utils;

@Service
public class ProjectServiceImpl {
	
	@Autowired
	private ProjectRepository projectRepository;
	
	@Autowired
	private TeamMemberRepository teamMemberRepository;
	
	@Autowired
	private TeamRepository teamRepository;
	
	@Autowired
	private UserRepository userRepository;
	
	@Autowired
	private TaskRepository taskRepository;
	
	
	private static final String CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private static final int RANDOM_LENGTH = 18;

    public static String generateProjectCode() {
        StringBuilder teamCode = new StringBuilder(23);
        SecureRandom secureRandom = new SecureRandom(); 

        SimpleDateFormat dateFormat = new SimpleDateFormat("MMddyy");
        String datePart = dateFormat.format(new Date()); 
        
        teamCode.append("project");
        String dateSubPart = datePart.substring(0, 5); 
        
        teamCode.append(dateSubPart);

        for (int i = 0; i < RANDOM_LENGTH; i++) {
            int index = secureRandom.nextInt(CHARACTERS.length());
            teamCode.append(CHARACTERS.charAt(index));
        }

        return teamCode.toString();
    }
	
	public Response getAllProjectByUserId(Long userid) {
		Response response = new Response();
		
		try {
			if(userid==null) {
				throw new OurException("Unable to find teams");
			}
			
			List<TeamMembers> teamMembers = teamMemberRepository.findByUserId(userid);
			List<Project> projectList = new ArrayList<>();
			List<TeamDTO> team = new ArrayList<>();
			List<ProjectDTO> projectDTOS = new ArrayList<>();
			
			for(TeamMembers members:teamMembers) {
				
				try {
					projectList.addAll(projectRepository.findByTeamId(members.getTeam().getId()));
					
					
				}
				catch(Exception e) {
					System.out.println("No projects for the current team");
				}
				
			}
			
			for (Project project : projectList) {
				
				List<Task> task = taskRepository.findByProjectId(project.getId());
				
				team.add(Utils.mapTeamEntityToTeamDto(teamRepository.findById(project.getTeam().getId()).orElseThrow(()->new OurException("No Team for this project"))));
				
	            ProjectDTO projectDTO = Utils.mapProjectEntityToProjectDto(project);
	            
	            if(!task.isEmpty()) {
					int totalTask = task.size();
					int completedTask = 0;
					if(task.size()>0) {
						for(Task t:task) {
							String s = t.getStatus();
							if(s.equals("completed")) {
								completedTask+=1;
							}
						}
					}
					
					int progress = (int)((completedTask * 100.0) / totalTask);
					projectDTO.setProjectprogress(progress);
					
					if(progress==100) {
						projectDTO.setStatus("Completed");
					}
				
				}
	            
	            long projectTeamSize = teamMemberRepository.countByTeamId(project.getTeam().getId());
	            projectDTO.setProjectteamsize((int) projectTeamSize);

	            projectDTOS.add(projectDTO);
	        }
			response.setTeamList(team);
			response.setProjectList(projectDTOS);
			response.setMessage("success");
			response.setStatusCode(200);
			
			
		}
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		return response;
	}
	
	
	public Response getAllProjectByteamId(Long userId,Long teamId) {
		Response response = new Response();
		
		try {
			if(teamId==null) {
				throw new OurException("Unable to find projects");
			}
			
			if(userId==null) {
				response.setStatusCode(200);
				response.setMessage("invalid");
				return response;
			}
			
			List<Project> projectList = projectRepository.findByTeamId(teamId);
			
			List<ProjectDTO> projectDTOS = new ArrayList<>();
			
			for (Project project : projectList) {
				List<Task> task = taskRepository.findByProjectId(project.getId());
	            ProjectDTO projectDTO = Utils.mapProjectEntityToProjectDto(project);
	            
	            if(!task.isEmpty()) {
					int totalTask = task.size();
					int completedTask = 0;
					if(task.size()>0) {
						for(Task t:task) {
							String s = t.getStatus();
							if(s.equals("completed")) {
								completedTask+=1;
							}
						}
					}
					
					int progress = (int)((completedTask * 100.0) / totalTask);
					projectDTO.setProjectprogress(progress);
					
					if(progress==100) {
						projectDTO.setStatus("Completed");
					}
				
				}
	            
	            long projectTeamSize = teamMemberRepository.countByTeamId(project.getTeam().getId());
	            projectDTO.setProjectteamsize((int) projectTeamSize);
	            
	            projectDTOS.add(projectDTO);
	        }
			
			
			
			response.setProjectList(projectDTOS);
			response.setMessage("success");
			response.setStatusCode(200);
			
			
		}
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		return response;
	}

	public Response addProject(Long userId,Long teamId,AddProjectDTO addProjectDTO) {
		Response response = new Response();
		Project project = new Project();
		
		if(userId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("User or team not found");
			return response;
		}
		
		if(addProjectDTO.getProjectdescription()==null || addProjectDTO.getEnddate()==null || addProjectDTO.getPriority()==null || addProjectDTO.getProjectname()==null || addProjectDTO.getStartdate()==null) {
			response.setStatusCode(400);
			response.setMessage("Fill all fields");
			return response;
		}
		
		if(addProjectDTO.getEnddate().isBefore(addProjectDTO.getStartdate())) {
			response.setStatusCode(400);
			response.setMessage("check start-date and end-date");
			return response;
		}
		
		try {
			
			if(teamRepository.existsByIdAndUserId(teamId, userId)) {
				
				Team team = teamRepository.findById(teamId).orElseThrow(()->new OurException("Team not found"));
				User user = userRepository.findById(userId).orElseThrow(()->new OurException("User not found"));
				
				project.setTeam(team);
				project.setUser(user);
				project.setEnddate(addProjectDTO.getEnddate());
				project.setPriority(addProjectDTO.getPriority());
				project.setProjectdescrption(addProjectDTO.getProjectdescription());
				project.setProjectname(addProjectDTO.getProjectname());
				project.setRole("ADMIN");
				project.setStartdate(addProjectDTO.getStartdate());
				project.setStatus("on going");
				
				String projectCode = generateProjectCode();
				
				project.setProjectcode(projectCode);
				
				projectRepository.save(project);
				
				response.setStatusCode(200);
				response.setMessage("success");
				return response;
			}
			else {
				response.setStatusCode(400);
				response.setMessage("You can't add project to this team");
				return response;
			}
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		return response;
	}

	public Response getEachProjectByProjId(Long userId, Long teamId, Long projectId) {
		
		Response response = new Response();
		
		if(userId==null || teamId==null || projectId==null) {
			response.setStatusCode(400);
			response.setMessage("invalid");
			return response;
		}
		
		try {
			
			Project project = projectRepository.findByIdAndTeamId(projectId,teamId).orElseThrow(()->new OurException("no such project exist"));
			
			ProjectDTO projectDto = Utils.mapProjectEntityToProjectDto(project);
			
			List<Task> task = taskRepository.findByProjectId(project.getId());
			if(!task.isEmpty()) {
				int totalTask = task.size();
				int completedTask = 0;
				if(task.size()>0) {
					for(Task t:task) {
						String s = t.getStatus();
						if(s.equals("completed")) {
							completedTask+=1;
						}
					}
				}
				
				int progress = (int)((completedTask * 100.0) / totalTask);
				projectDto.setProjectprogress(progress);
				
				if(progress==100) {
					projectDto.setStatus("Completed");
				}
			
			}
			
			response.setStatusCode(200);
			response.setMessage("success");
			response.setProjectDto(projectDto);
			
		}
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		return response;
	}

}
