package com.task.service.impl;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.task.dto.AddTask;
import com.task.dto.ProfilePictures;
import com.task.dto.Response;
import com.task.dto.TaskDTO;
import com.task.dto.UserDTO;
import com.task.entity.Project;
import com.task.entity.Task;
import com.task.entity.TaskMember;
import com.task.entity.Team;
import com.task.entity.User;
import com.task.exception.OurException;
import com.task.repository.ProjectRepository;
import com.task.repository.TaskMemberRepository;
import com.task.repository.TaskRepository;
import com.task.repository.TeamMemberRepository;
import com.task.repository.TeamRepository;
import com.task.repository.UserRepository;
import com.task.utils.Utils;

@Service
public class TaskServiceImpl {

	@Autowired
	private TaskRepository taskRepository;
	
	@Autowired
	private UserRepository userRepository;
	
	@Autowired
	private TaskMemberRepository taskMemberRepository;
	
	@Autowired
	private TeamRepository teamRepository;
	
	@Autowired
	private ProjectRepository projectRepository;
	
	@Autowired
	private TeamMemberRepository teamMemberRepository;

	
	public Response getAllTaskByuserId(Long userId) {
		
		Response response = new Response();
		
		if(userId==null || userId<0) {
			response.setStatusCode(400);
			response.setMessage("invalid user");
			return response;
		}
		try {
			
			List<TaskMember> taskAssign = taskMemberRepository.findByUserId(userId);
			
			List<TaskDTO> taskDTOs = new ArrayList<>();
			
			if(!taskAssign.isEmpty()) {
				
				if(!taskAssign.isEmpty()) {
				for(TaskMember t1:taskAssign) {		
					User user1 = userRepository.findById(t1.getUser().getId()).orElseThrow(()->new OurException("Assigned to no one"));
					TaskDTO task1 = Utils.mapTaskEntityToTaskDto(t1.getTask());
					List<TaskMember> taskmembers = taskMemberRepository.findByTaskId(t1.getTask().getId());
					List<String> pics = new ArrayList<>();
					
					for(TaskMember tas:taskmembers) {
						pics.add((Utils.mapUserEntityToUserDTO(tas.getUser())).getProfilepicture());
					}
					
					task1.setAssignedUser(pics);
					task1.setAssignedto(user1.getEmail());
					task1.setJobtype(t1.getTask().getJobtype());
					taskDTOs.add(task1);
				}
			}
				
				response.setTaskList(taskDTOs);
				response.setStatusCode(200);
				response.setMessage("success");
				
			}
			else {
				response.setStatusCode(400);
				response.setMessage("No task is available");
				return response;
			}
			
		}
		catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
	}

	public Response getAllTaskByProjectId(Long userId, Long teamId, Long projectId) {
		
		Response response = new Response();
		
		if(userId==null || teamId==null || projectId==null) {
			
			response.setStatusCode(400);
			response.setMessage("Invalid");
			return response;
			
		}
		
		try {
			
			List<TaskMember> taskAssign = taskMemberRepository.findByUserIdAndProjectId(userId,projectId);
			
			List<TaskDTO> taskDTOs = new ArrayList<>();
			
			if(!taskAssign.isEmpty()) {
				
				if(!taskAssign.isEmpty()) {
				for(TaskMember t1:taskAssign) {					
					User user1 = userRepository.findById(t1.getUser().getId()).orElseThrow(()->new OurException("Assigned to no one"));
					TaskDTO task1 = Utils.mapTaskEntityToTaskDto(t1.getTask());
					List<TaskMember> taskmembers = taskMemberRepository.findByTaskId(t1.getTask().getId());
					List<String> pics = new ArrayList<>();
					
					for(TaskMember tas:taskmembers) {
						pics.add((Utils.mapUserEntityToUserDTO(tas.getUser())).getProfilepicture());
					}
					
					task1.setAssignedUser(pics);
					task1.setAssignedto(user1.getEmail());
					task1.setJobtype(t1.getTask().getJobtype());
					taskDTOs.add(task1);
				}
				}
				
				
				response.setTaskList(taskDTOs);
				response.setStatusCode(200);
				response.setMessage("success");
				
			}
			else {
				response.setStatusCode(400);
				response.setMessage("No task is available");
				return response;
			}
			
			
		}
		
		catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
	}

	public Response getTodayTasks(Long userId) {
		Response response = new Response();
		
		if(userId==null || userId<0) {
			response.setStatusCode(400);
			response.setMessage("invalid user");
			return response;
		}
		try {
			
			
			List<TaskMember> taskAssign = taskMemberRepository.findByUserId(userId);
			
			List<TaskDTO> taskDTOs = new ArrayList<>();
			
			LocalDateTime today = LocalDateTime.now();
			
			if(!taskAssign.isEmpty()) {
				
				if(!taskAssign.isEmpty()) {
				for(TaskMember t1:taskAssign) {					
					User user1 = userRepository.findById(t1.getUser().getId()).orElseThrow(()->new OurException("Assigned to no one"));
					if(!t1.getTask().getStatus().equals("completed")) {
						if(today.isAfter(t1.getTask().getStartdate()) && today.isBefore(t1.getTask().getEnddate())) {
						TaskDTO task1 = Utils.mapTaskEntityToTaskDto(t1.getTask());
						List<TaskMember> taskmembers = taskMemberRepository.findByTaskId(t1.getTask().getId());
						List<String> pics = new ArrayList<>();
						
						for(TaskMember tas:taskmembers) {
							pics.add((Utils.mapUserEntityToUserDTO(tas.getUser())).getProfilepicture());
						}
						
						task1.setAssignedUser(pics);
						task1.setAssignedto(user1.getEmail());
						task1.setJobtype(t1.getTask().getJobtype());
						taskDTOs.add(task1);
						}
					}	
				}
				}
				
				
				response.setTaskList(taskDTOs);
				response.setStatusCode(200);
				response.setMessage("success");
				
			}
			else {
				response.setStatusCode(400);
				response.setMessage("No task is available");
				return response;
			}
			
		}
		catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
	}
	
	public Response addTaskTOProject(Long userid,Long projectid,Long teamid,AddTask task) {
		
		Response response = new Response();
		
		Task tasks = new Task();
		
		if(userid==null || projectid==null || teamid==null) {
			
			response.setStatusCode(400);
			response.setMessage("Invalid");
			return response;
			
		}
		
		try {
			
			if(!teamRepository.existsByIdAndUserId(teamid, userid)) {
				
				response.setStatusCode(400);
				response.setMessage("You can't add task to this team");
				return response;
				
			}
			
			if(!projectRepository.existsByIdAndUserIdAndTeamId(projectid, userid, teamid)) {
				
				response.setStatusCode(400);
				response.setMessage("There is no such project exist");
				return response;
				
			}
			
			if(task.getTaskname()==null || task.getTaskdescription()==null || task.getPriority()==null || task.getJobtype()==null) {
				response.setStatusCode(400);
				response.setMessage("Check all the fields");
				return response;
			}
			
			if(task.getStartdate().isAfter(task.getEnddate())) {
				response.setStatusCode(400);
				response.setMessage("Check your startdate and enddate");
				return response;
			}
			
			Project project = projectRepository.findById(projectid).orElseThrow(()->new OurException("No such project"));
			
			Team team = teamRepository.findById(teamid).orElseThrow(()->new OurException("No such team exist"));
			
			User user = userRepository.findById(userid).orElseThrow(()->new OurException("User not found"));
			
			LocalDateTime today = LocalDateTime.now();
			
			System.out.println(task.getAssignto());
			tasks.setTaskname(task.getTaskname());
			tasks.setTaskdescription(task.getTaskdescription());
			tasks.setPriority(task.getPriority());
			tasks.setJobtype(task.getJobtype());
			tasks.setStartdate(task.getStartdate());
			tasks.setEnddate(task.getEnddate());
			if(task.getStartdate().isAfter(today)) {
				tasks.setStatus("Not yet started");
			}
			else if(task.getStartdate().isBefore(today)) {
				tasks.setStatus("on going");
			}
			
			tasks.setUser(user);
			tasks.setTeam(team);
			tasks.setProject(project);
			
			Task taskss =taskRepository.save(tasks);
			
			TaskMember taskMember = new TaskMember();
			
			
			taskMember.setProject(project);
			taskMember.setTask(taskss);
			taskMember.setUser(user);
			taskMember.setTeam(team);
			
			taskMemberRepository.save(taskMember);
			
			for(String t:task.getAssignto()) {
				
				if(t!=null) {
				TaskMember mem = new TaskMember();
				User users = userRepository.findByEmail(t).orElseThrow(()->new OurException("User Not Found"));
				
				if(teamMemberRepository.existsByUserIdAndTeamId(users.getId(), teamid)) {
					mem.setProject(project);
					mem.setTask(taskss);
					mem.setTeam(team);
					mem.setUser(users);
					
					taskMemberRepository.save(mem);
				}}
				
			}
			
			response.setStatusCode(200);
			response.setMessage("Success");
			return response;	
			
		}
		
		catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
		
	}

	@Transactional
	public Response completeTask(Long taskId, Long teamId, Long projectId) {
		
		Response response = new Response();
		
		if(taskId==null || teamId==null || projectId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid");
			return response;
		}
		
		try {
			
			if(taskRepository.existsByIdAndTeamIdAndProjectId(taskId,teamId,projectId)) {
				String status = "completed";
				taskRepository.updateStatus(status,taskId,teamId,projectId);
				response.setStatusCode(200);
				response.setMessage("Success");
				return response;
			}
			else {
				response.setStatusCode(400);
				response.setMessage("No such task exist");
				return response;
			}
			
			
		}
		
		catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
		
	}
	
}
